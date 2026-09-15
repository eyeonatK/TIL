# 환경 변수 관리 (Environment Variables & Pydantic Settings)

민감한 설정 정보를 코드 외부(`.env`)로 분리하고, `pydantic-settings`와 `SecretStr`을 활용해 안전하게 주입·검증·마스킹하는 기법 정리.

---

## 1. 환경 변수(Environment Variable) 관리 원칙

* **하드코딩 금지**: 보안 정보나 실행 환경별로 달라지는 설정값은 소스 코드에 직접 작성하지 않는다.
* **저장소 커밋 금지**: 실제 민감 정보를 포함한 `.env` 파일은 절대 Git에 올리지 않는다 (`.gitignore` 등록 필수).
* **`.env.example` 동시 관리**: 협업자를 위해 실제 값 대신 키 이름과 형식 가이드만 담은 템플릿 파일을 형상 관리에 포함한다.

---

## 2. Settings 구성 및 보안 타입 (`SecretStr`)

`SecretStr`을 적용하면 `print()` 출력이나 로깅 시 키 값이 노출되지 않고 `**********` 형태로 자동 마스킹 처리된다.

```bash
# 필수 패키지 설치
python -m pip install pydantic-settings
```

### 2.1 설정 파일 준비

```bash
# .env (로컬 개발용)
APP_MODE=mock
ANTHROPIC_API_KEY=your-secret-api-key
TOP_K=3
MAX_TOKENS=400
DAILY_CALL_LIMIT=200
```

### 2.2 중앙 집중식 Settings 클래스 구현

```python
from functools import lru_cache
import os
from pydantic import Field, SecretStr
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        extra="ignore",        # .env에 정의되지 않은 추가 변수 무시
        case_sensitive=False   # APP_MODE와 app_mode를 동일하게 매핑
    )

    # 실행 모드 제약: "mock" 또는 "live"만 허용
    app_mode: str = Field(default="mock", pattern=r"^(mock|live)$")

    # 보안 민감값: 콘솔 및 로그 노출 차단 (미설정 허용)
    anthropic_api_key: SecretStr | None = None
    upstage_api_key: SecretStr | None = None

    # 모델 하이퍼파라미터 및 수치 제약 조건
    top_k: int = Field(default=3, ge=1, le=20)
    max_tokens: int = Field(default=400, ge=1, le=8192)
    temperature: float = Field(default=0.0, ge=0.0, le=1.0)
    daily_call_limit: int = Field(default=200, ge=1)
    max_input_chars: int = Field(default=200, ge=1)

    # 애플리케이션 공통 플래그
    database_url: str = "sqlite:///./app.db"
    debug: bool = False
    allow_external_send: bool = False

    # 도메인 편의 프로퍼티 (live 모드 여부 확인)
    @property
    def is_live(self) -> bool:
        return self.app_mode == "live"

# 싱글톤 인스턴스 반환 함수
@lru_cache
def get_settings() -> Settings:
    return Settings()
```

---

## 3. 사용 및 보안 마스킹 검증

```python
settings = get_settings()

# 1. 일반 필드 조회
print(f"실행 모드: {settings.app_mode}")
print(f"Live 모드 여부: {settings.is_live}")
print(f"Top-K: {settings.top_k}")

# 2. SecretStr 마스킹 동작 확인
# print() 호출 시: ********** (보안 유지)
print(f"API 키 노출 방지: {settings.anthropic_api_key}")

# 3. 실제 원본 값이 필요할 때만 명시적 호출
if settings.anthropic_api_key:
    raw_key = settings.anthropic_api_key.get_secret_value()
```

---

## 4. 환경 변수 초기화 및 실무 베스트 프랙티스

단위 테스트나 환경 전환 시 기존 로드된 환경 변수를 안전하게 소거한다.

```python
# 환경 변수 단일 / 다중 삭제
keys_to_clear = ["APP_MODE", "ANTHROPIC_API_KEY"]
for key in keys_to_clear:
    os.environ.pop(key, None)  # 키가 없어도 KeyError 방지
```

### 실무 주의사항 체크리스트

| 항목 | 나쁜 예 (Anti-Pattern) | 좋은 예 (Best Practice) |
| :--- | :--- | :--- |
| **호출 위치** | 각 모듈에서 `os.getenv("TOP_K")` 개별 호출 | `get_settings().top_k` 중앙 집중 참조 |
| **민감 정보** | `api_key: str` (로그 노출 위험) | `api_key: SecretStr` (자동 마스킹) |
| **기본값 설정** | `debug: bool = True` (보안 취약) | `debug: bool = False` (보안 강화) |
| **설정 소거** | `del os.environ["KEY"]` (미존재 시 크래시) | `os.environ.pop("KEY", None)` |