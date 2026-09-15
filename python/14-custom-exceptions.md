# 도메인 예외 계층 설계 (Custom Exception Hierarchy)

API 상태 코드(HTTP Status)와 비즈니스 에러 코드를 구조화하고, `raise ... from ...`을 통해 원인 추적성을 보존하는 예외 설계 패턴 정리.

---

## 1. 예외 계층화의 필요성

* **비즈니스 에러 정형화**: 단순 `Exception` 대신 도메인 상황에 맞춘 커스텀 예외를 생성하여 HTTP 상태 코드와 식별 코드를 일관되게 반환한다.
* **원인 추적성 유지**: 외부 라이브러리 예외를 래핑할 때 본래 에러 정보를 유실하지 않도록 예외 체이닝을 구성한다.

---

## 2. 기본 구조 및 상속 트리

애플리케이션의 모든 커스텀 예외가 상속받는 최상위 루트 예외(`AgentError`)를 두고 세부 예외를 파생시킨다.

```text
Exception (파이썬 내장 루트)
   └── AgentError (우리 앱의 도메인 루트, 400)
         ├── NotFound (404)
         ├── PermissionDenied (403)
         ├── ValidationFailed (422)
         ├── GuardTripped (400)
         ├── RateLimited (429)
         └── ExternalServiceError (502)
```

### 2.1 도메인 예외 계층 코드 (`app/core/exceptions.py`)

```python
class AgentError(Exception):
    """애플리케이션 도메인 최상위 예외"""
    status_code: int = 400
    code: str = "agent_error"

    def __init__(self, message: str, *, detail: str | None = None) -> None:
        super().__init__(message)
        self.message = message
        self.detail = detail

# 404: 요청 자원 미존재
class NotFound(AgentError):
    status_code = 404
    code = "not_found"

# 403: 접근 권한 부족
class PermissionDenied(AgentError):
    status_code = 403
    code = "permission_denied"

# 422: 비즈니스 입력값 유효성 실패
class ValidationFailed(AgentError):
    status_code = 422
    code = "validation_failed"

# 400: 프롬프트 인젝션 등 가드레일 위배
class GuardTripped(AgentError):
    status_code = 400
    code = "guard_tripped"

# 429: API 호출 한도 초과
class RateLimited(AgentError):
    status_code = 429
    code = "rate_limited"

# 502: 외부 연동 실패 (LLM, 외부 API)
class ExternalServiceError(AgentError):
    status_code = 502
    code = "external_service_error"
```

---

## 3. 원본 스택 보존 (`raise ... from ...`)

외부 네트워크 오류나 라이브러리 예외를 도메인 예외로 변환할 때 `from e`를 붙여 에러의 발생 근원지를 추적할 수 있도록 체이닝한다.

```python
def fetch_external_data(doc_id: str) -> str:
    try:
        # 외부 통신 작업 수행 중 오류 발생 가정
        raise ConnectionError("Connection refused by host")
    except ConnectionError as e:
        # 원인 예외(e)를 체이닝하여 우리 도메인 예외로 전환
        raise ExternalServiceError(
            "문서 변환 외부 서비스에 연결하지 못했습니다.",
            detail=str(e)
        ) from e
```

---

## 4. 전역 예외 핸들러 패턴 (Global Handler)

컨트롤러나 서비스에서 발생한 에러를 수신하여 정형화된 JSON 응답 딕셔너리로 변환한다.

```python
def error_response_handler(exc: Exception) -> dict:
    # 1. 애플리케이션이 의도한 도메인 예외인 경우
    if isinstance(exc, AgentError):
        return {
            "status": exc.status_code,
            "code": exc.code,
            "message": exc.message,
            "detail": exc.detail
        }

    # 2. 시스템 에러 등 예측하지 못한 심각한 런타임 오류
    return {
        "status": 500,
        "code": "internal_server_error",
        "message": "서버 내부 오류가 발생했습니다.",
        "detail": None
    }
```