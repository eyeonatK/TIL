# 파이썬 엔터프라이즈 로깅 (Enterprise Logging)

`print()`를 대체하여 로그 레벨 제어, 표준 출력 포맷팅, 노이즈 필터링 및 단일 루트 로거(Root Logger) 기반의 중앙 집중식 로깅 아키텍처 구축 정리.

---

## 1. Print vs Logging

| 비교 항목 | `print()` | `logging` 모듈 |
| :--- | :--- | :--- |
| **로그 레벨 제어** | 불가 (무조건 출력) | `DEBUG`부터 `CRITICAL`까지 단계별 필터링 가능 |
| **메타데이터 기록** | 수동 작성 필요 | 시각(`asctime`), 파일 모듈 경로(`name`), 레벨 자동 기록 |
| **출력 타깃 전환** | 표준 출력(stdout) 고정 | 콘솔, 로테이션 파일, 외부 APM/ELK 시스템 전송 용이 |
| **에러 추적성** | 텍스트만 출력 | `logger.exception()`으로 전체 스택 트레이스 자동 수집 |

---

## 2. 로그 레벨 체계

로그 레벨은 숫자가 클수록 심각도가 높으며, **설정한 레벨 이상의 로그만 출력**된다.

```text
DEBUG (10) < INFO (20) < WARNING (30) < ERROR (40) < CRITICAL (50)
```

| 레벨 | 수치 | 용도 및 상황 예시 |
| :---: | :---: | :--- |
| **DEBUG** | 10 | 변수 상태, SQL 쿼리 등 개발/디버깅 중에만 확인할 상세 정보 |
| **INFO** | 20 | 문서 인덱싱 시작/종료, 사용자 로그인, 정상적인 비즈니스 이벤트 |
| **WARNING** | 30 | API 호출 한도 80% 근접, 재시도 발생 등 잠재적 장애 경고 |
| **ERROR** | 40 | 외부 LLM 호출 타임아웃, DB 연결 실패 등 특정 기능의 수행 실패 |
| **CRITICAL** | 50 | 데이터베이스 영구 락, 디스크 풀 등 전체 서버 중단 위기 |

---

## 3. 엔터프라이즈 로깅 아키텍처 (`backend/app/core/logging.py`)

FastAPI 및 서드파티 라이브러리가 혼재된 실무 환경에서는 `logging.basicConfig()`가 무시될 수 있으므로, **루트 로거를 직접 초기화하고 서드파티 노이즈 로거를 제어**하는 멱등성 셋업을 구축한다.

```python
import logging
import sys

# 중복 설정 방지 플래그
_CONFIGURED: bool = False

# 표준 출력 포맷 (시:분:초 | 레벨(8자리정렬) | 모듈경로: 메시지)
_FORMAT: str = "%(asctime)s %(levelname)-8s %(name)s: %(message)s"
_DATEFMT: str = "%H:%M:%S"

# 대량의 연결 로그를 쏟아내는 서드파티 로거 목록
_NOISY_LOGGERS = ("httpx", "httpcore", "urllib3", "asyncio")

def setup_logging(level: int = logging.INFO, stream=None) -> None:
    """애플리케이션 전역 로깅 환경을 단 1회 초기화한다."""
    global _CONFIGURED
    if _CONFIGURED:
        return

    # 1. 출력 스트림 핸들러 및 포매터 설정
    handler = logging.StreamHandler(stream or sys.stdout)
    handler.setFormatter(logging.Formatter(_FORMAT, datefmt=_DATEFMT))

    # 2. 최상위 루트(Root) 로거 획득 및 핸들러 단일화
    root = logging.getLogger()
    root.setLevel(level)
    root.handlers = [handler]  # 기존 기본 핸들러를 덮어씌워 중복 출력 방지

    # 3. 비즈니스 로그를 가리는 서드파티 로거 레벨 상향 조정
    for name in _NOISY_LOGGERS:
        logging.getLogger(name).setLevel(logging.WARNING)

    _CONFIGURED = True

def get_logger(name: str) -> logging.Logger:
    """모듈별 로거를 반환하며, 사전 설정이 안 되어 있다면 자동으로 초기화한다."""
    setup_logging()
    return logging.getLogger(name)
```

---

## 4. 계층별 로거 사용법

모든 하위 서비스 및 라우터 모듈에서는 설정을 반복하지 않고 `get_logger(__name__)`을 호출해 사용한다.

```python
# backend/app/services/document_service.py
from app.core.logging import get_logger
from app.core.exceptions import ExternalServiceError

logger = get_logger(__name__)

def ingest_document(doc_id: str) -> str:
    # f-string 대신 %s 바인딩 사용 (지연 평가로 불필요한 연산 방지)
    logger.info("문서 적재 시작: %s", doc_id)

    try:
        # 비즈니스 로직 수행
        return f"{doc_id} 내용"
    except ConnectionError as e:
        # exception()을 사용하면 에러 메시지와 함께 원본 트레이스백이 기록됨
        logger.exception("외부 LLM 변환 실패: %s", doc_id)
        raise ExternalServiceError("문서 연동 실패", detail=str(e)) from e
```

---

## 5. 실무 필수 수칙 (Security & Best Practices)

* **민감 정보(PII / Secret) 마스킹**: 비밀번호, 세션 토큰, 주민등록번호, API Secret 등은 절대 로그에 남겨서는 안 된다 (`SecretStr` 활용).
* **문자열 포매팅 안티패턴 지양**:
  * 나쁜 예: `logger.debug(f"연산 결과: {heavy_calculation()}")` (로그 레벨 미달이어도 연산 수행됨)
  * 좋은 예: `logger.debug("연산 결과: %s", heavy_calculation)` (지연 평가)
* **모듈 식별자 명시**: `getLogger("demo")`와 같은 하드코딩 대신 항상 `getLogger(__name__)`을 사용하여 어느 계층, 어느 파일에서 발생한 로그인지 추적 경로를 남긴다.