# FastAPI 기초 및 요청 파라미터

FastAPI와 Uvicorn의 역할, API Endpoint 작성법, Path/Query Parameter 및 `Query()`를 이용한 요청 데이터 검증 정리.

---

## 1. FastAPI와 Uvicorn

**FastAPI**는 Python으로 웹 API 서버를 만드는 웹 프레임워크이다.

**Uvicorn**은 포트를 열어 HTTP 요청을 받고 FastAPI 애플리케이션에 전달하는 서버이다.

```text
Client
   │
   │ HTTP Request
   ▼
Uvicorn
   │
   ▼
FastAPI
   │
   ├── Service
   ├── PostgreSQL
   ├── RAG
   └── AI Agent
```

즉 두 기술의 역할은 다음과 같이 구분한다.

```text
FastAPI
→ 어떤 요청을 어떤 Python 코드로 처리할지 정의

Uvicorn
→ HTTP 요청을 받아 FastAPI 애플리케이션에 전달
```

---

## 2. 첫 FastAPI Endpoint

FastAPI 애플리케이션을 생성한다.

```python
from fastapi import FastAPI

app = FastAPI(
    title="사내 업무 에이전트 연습",
    version="0.1.0",
)


@app.get("/health")
def health() -> dict:
    return {"status": "ok"}
```

다음 데코레이터는 `GET /health` 요청과 `health()` 함수를 연결한다.

```python
@app.get("/health")
```

```text
GET
→ HTTP Method

/health
→ 요청 Path
```

클라이언트가 다음 요청을 보내면:

```http
GET /health
```

FastAPI는 `health()`를 실행하여 결과를 응답한다.

### Uvicorn 실행

```bash
uvicorn app.practice_fastapi:app --app-dir backend --reload --port 8000
```

각 옵션의 역할은 다음과 같다.

| 구성 | 의미 |
| :--- | :--- |
| `app.practice_fastapi:app` | 모듈 내부의 FastAPI `app` 객체 실행 |
| `--app-dir backend` | 모듈 탐색 시작 위치 |
| `--reload` | 코드 변경 시 서버 자동 재시작 |
| `--port 8000` | 8000번 Port에서 요청 수신 |

실행 후 FastAPI가 자동 생성한 Swagger UI를 통해 API를 확인할 수 있다.

```text
http://127.0.0.1:8000/docs
```

---

## 3. Path Parameter

Path Parameter는 URL 경로의 일부를 변수로 받아 여러 리소스를 하나의 Endpoint에서 처리하는 방법이다.

```python
@app.get("/documents/{doc_id}")
def get_document(doc_id: str) -> dict:
    return {"doc_id": doc_id}
```

다음 요청이 들어오면:

```http
GET /documents/DOC-HR-014
```

`DOC-HR-014`가 함수의 `doc_id`에 전달된다.

```text
/documents/DOC-HR-014
           │
           ▼
doc_id = "DOC-HR-014"
```

따라서 문서마다 별도의 Endpoint를 만들 필요가 없다.

### 고정 경로를 먼저 선언

고정 경로와 Path Parameter 경로가 함께 존재한다면 고정 경로를 먼저 선언한다.

```python
@app.get("/documents/latest")
def get_latest_document() -> dict:
    ...


@app.get("/documents/{doc_id}")
def get_document(doc_id: str) -> dict:
    ...
```

동적 경로가 먼저 선언되어 있으면 `latest`가 `doc_id`의 값으로 처리될 수 있기 때문이다.

```text
/documents/latest
           ↓
doc_id = "latest"
```

따라서 구체적인 고정 경로를 먼저 배치하고 동적 경로를 이후에 배치한다.

---

## 4. Query Parameter

Query Parameter는 Path 뒤에 `?`를 사용하여 조회 조건 등의 추가 데이터를 전달한다.

```text
/documents?dept=인사&file_format=pdf
```

구조는 다음과 같다.

```text
/documents
    │
    └── Path

dept=인사
file_format=pdf
    │
    └── Query Parameters
```

여러 Query Parameter는 `&`로 연결한다.

### Path Parameter와 Query Parameter

```text
Path Parameter
→ 특정 자원을 식별

/documents/DOC-HR-014


Query Parameter
→ 조회 조건 등을 추가

/documents?dept=인사&file_format=pdf
```

FastAPI에서는 함수의 매개변수를 이용하여 Query Parameter를 받을 수 있다.

```python
@app.get("/documents")
def get_documents(
    dept: str | None = None,
    file_format: str | None = None,
) -> dict:
    return {
        "dept": dept,
        "file_format": file_format,
    }
```

기본값을 `None`으로 설정하면 해당 Query Parameter를 생략할 수 있다.

```text
/documents
→ dept = None

/documents?dept=인사
→ dept = "인사"
```

반대로 기본값을 지정하지 않은 매개변수는 필수 Query Parameter가 된다.

---

## 5. Query()를 이용한 요청 범위 제한

`Query()`를 사용하면 Query Parameter의 기본값과 허용 범위를 지정할 수 있다.

```python
from fastapi import Query


@app.get("/documents")
def get_documents(
    limit: int = Query(default=20, ge=1, le=100),
) -> dict:
    return {"limit": limit}
```

각 제약 조건의 의미는 다음과 같다.

| 옵션 | 의미 |
| :--- | :--- |
| `default=20` | 생략 시 기본값 `20` |
| `ge=1` | 1 이상 |
| `le=100` | 100 이하 |

따라서 다음과 같이 동작한다.

```text
/documents
→ limit = 20

/documents?limit=10
→ limit = 10

/documents?limit=0
→ 허용 범위 미만

/documents?limit=101
→ 허용 범위 초과
```

이전에 Pydantic에서 사용했던 데이터 검증 개념이 HTTP 요청 파라미터 검증으로 연결된다.

---

## 6. 문서 필터링 API 실습

DB 대신 문서 리스트가 존재한다고 가정한다.

```python
documents = [
    {
        "doc_id": "DOC-HR-014",
        "title": "2026년 휴가 운영 규정",
        "dept": "인사",
        "security_level": "일반",
        "file_format": "docx",
        "status": "active",
    },
    {
        "doc_id": "DOC-HR-021",
        "title": "복리후생 운영 지침",
        "dept": "인사",
        "security_level": "일반",
        "file_format": "pdf",
        "status": "active",
    },
    {
        "doc_id": "DOC-SE-011",
        "title": "정보보안 관리 규정",
        "dept": "보안",
        "security_level": "3급",
        "file_format": "pdf",
        "status": "active",
    },
    {
        "doc_id": "DOC-PU-007",
        "title": "구매 계약 업무 지침",
        "dept": "구매",
        "security_level": "대외비",
        "file_format": "docx",
        "status": "active",
    },
]
```

선택적으로 전달된 Query Parameter만 이용하여 문서를 필터링한다.

```python
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/practice/documents")
def get_documents(
    dept: str | None = None,
    security_level: str | None = None,
    file_format: str | None = None,
    limit: int = Query(default=20, ge=1, le=100),
) -> list[dict]:
    result = documents

    if dept is not None:
        result = [
            doc
            for doc in result
            if doc["dept"] == dept
        ]

    if security_level is not None:
        result = [
            doc
            for doc in result
            if doc["security_level"] == security_level
        ]

    if file_format is not None:
        result = [
            doc
            for doc in result
            if doc["file_format"] == file_format
        ]

    return result[:limit]
```

각 조건은 이전 단계의 필터링 결과에 순차적으로 적용된다.

```text
전체 문서
   │
   │ dept=인사
   ▼
인사 문서
   │
   │ file_format=pdf
   ▼
인사 + PDF 문서
   │
   │ limit=10
   ▼
최대 10개 반환
```

따라서:

```text
/practice/documents
```

모든 문서를 조회하고,

```text
/practice/documents?dept=인사
```

인사 부서 문서를 조회하며,

```text
/practice/documents?dept=인사&file_format=pdf
```

여러 조건을 함께 전달하면 모든 조건을 만족하는 문서를 조회한다.

---

## 7. 핵심 정리

FastAPI에서는 HTTP의 Method와 Path를 Python 함수에 연결하여 API Endpoint를 정의한다.

```text
HTTP Request
      │
      ▼
Uvicorn
      │
      ▼
FastAPI Router
      │
      ├── Method
      │     GET
      │
      ├── Path
      │     /documents/{doc_id}
      │
      ├── Path Parameter
      │     doc_id
      │
      └── Query Parameter
            dept / file_format / limit
      │
      ▼
Python 함수 실행
      │
      ▼
HTTP Response
```

핵심 원칙은 다음과 같다.

- FastAPI는 Python 기반 웹 API 프레임워크이다.
- Uvicorn은 HTTP 요청을 받아 FastAPI 애플리케이션에 전달한다.
- Path Parameter는 특정 리소스를 식별할 때 사용한다.
- 고정 Path는 동적 Path보다 먼저 선언한다.
- Query Parameter는 검색이나 필터링 등의 추가 조건을 전달한다.
- 기본값이 없는 Query Parameter는 필수 값이 된다.
- 선택적인 값은 `str | None = None`과 같이 표현할 수 있다.
- `Query()`를 사용하여 기본값과 값의 범위를 제한할 수 있다.