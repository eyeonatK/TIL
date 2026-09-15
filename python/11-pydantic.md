# Pydantic 데이터 검증 및 직렬화 (Validation & Serialization)

파이썬 타입 힌트를 기반으로 데이터의 런타임 유효성을 검증하고 DTO(Data Transfer Object) 모델을 설계하는 라이브러리 정리.

---

## 1. Pydantic 핵심 개념

* **타입 힌트 기반 검증**: 선언된 타입과 규칙에 맞지 않는 데이터가 유입되면 `ValidationError`를 발생시킨다.
* **자동 형변환 (Type Coercion)**: 웹 요청으로 유입된 문자열 숫자(`"100"`) 등 호환 가능한 타입은 정의된 타입(`int`)으로 자동 캐스팅한다.

```bash
# 패키지 설치
python -m pip install pydantic
```

---

## 2. Field 제약 조건 및 열거형(Enum)

`Field()` 함수를 사용하여 단순 타입 이상의 세부 제약 규칙을 설정한다.

| 속성 | 설명 | 예시 |
| :--- | :--- | :--- |
| `min_length`, `max_length` | 문자열/리스트의 최소·최대 길이 제한 | `Field(..., min_length=2, max_length=50)` |
| `ge`, `gt`, `le`, `lt` | 숫자 값의 이상, 초과, 이하, 미만 제약 | `Field(default=1, ge=1, le=100)` |
| `pattern` | 정규표현식(Regex)을 통한 포맷 제약 | `Field(pattern=r"^DOC-\d{3}$")` |
| `default` | 기본값 지정 (선택 필드로 전환) | `Field(default="pending")` |
| `description` | 필드 설명 메타데이터 (FastAPI 문서 자동화에 반영) | `Field(description="사용자 닉네임")` |

```python
from enum import Enum
from pydantic import BaseModel, Field

class Role(str, Enum):
    ADMIN = "관리자"
    USER = "일반사용자"

class Account(BaseModel):
    account_id: int
    nickname: str = Field(..., min_length=2, max_length=10)
    role: Role = Role.USER
```

---

## 3. 모델 직렬화 및 역직렬화 (Serialization)

API 서버와 클라이언트 간에 데이터를 주고받을 때 상호 변환 메서드를 사용한다.

| 메서드 | 변환 방향 | 설명 |
| :--- | :--- | :--- |
| `model.model_dump()` | **Model 객체 → Python dict** | 내부 로직이나 DB 매핑 시 활용 |
| `model.model_dump_json()` | **Model 객체 → JSON 문자열** | 클라이언트로 응답 반환 시 활용 |
| `Model.model_validate(dict)` | **Python dict → Model 객체** | 딕셔너리 데이터를 모델로 검증 및 복원 |
| `Model.model_validate_json(json)` | **JSON 문자열 → Model 객체** | 네트워크로 수신된 JSON을 모델로 역직렬화 |

```python
# Model -> JSON 문자열
doc = Account(account_id=1, nickname="tester")
json_str = doc.model_dump_json(indent=2)

# JSON 문자열 -> Model 복원
restored_doc = Account.model_validate_json(json_str)
```

---

## 4. 유효성 검사기 (Field Validator)

`@field_validator` 데코레이터를 통해 데이터 저장 전 전처리 및 커스텀 검증을 수행한다.

```python
from pydantic import BaseModel, field_validator

class Document(BaseModel):
    code: str
    title: str

    # 데이터 유입 즉시 대문자로 전처리 (before 모드)
    @field_validator("code", mode="before")
    @classmethod
    def to_uppercase(cls, v: str) -> str:
        return v.upper() if isinstance(v, str) else v

    # 문자열 앞뒤 공백 제거
    @field_validator("title")
    @classmethod
    def strip_whitespace(cls, v: str) -> str:
        return v.strip()
```

---

## 5. DTO 설계 실무 베스트 프랙티스

* **요청(Create)과 응답(Out) 분리**: 입력받는 데이터 형태(`DocumentCreate`)와 서버에서 내보내는 응답 데이터 형태(`DocumentOut`)를 별도 모델로 분리하여 민감 정보 노출을 차단한다.
* **딕셔너리 응답 지양**: API 엔드포인트 반환형으로 `dict` 대신 검증된 `BaseModel` 인스턴스를 사용하여 스키마 계약(Contract)을 보장한다.
* **선택 필드 남발 금지**: `str | None` 형태를 과도하게 허용하면 데이터 무결성을 보장받지 못하므로 필수 필드는 엄격히 지정한다.