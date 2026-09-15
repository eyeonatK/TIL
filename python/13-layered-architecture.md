# 백엔드 아키텍처 및 계층형 설계 (Layered Architecture)

단일 파일 집중(Monolithic script) 안티 패턴을 탈피하고, 관심사 분리(Separation of Concerns)를 실현하기 위한 계층형 아키텍처 구조 정리.

---

## 1. 계층형 아키텍처 개요

모든 비즈니스 로직, DB 접근, 통신 처리를 단일 파일(`main.py`)에 모아두면 결합도가 높아져 유지보수와 테스트가 불가능해진다. 요청부터 응답까지 책임을 명확히 나누어 격리한다.

### 단일 책임 흐름도
```text
[ Client (Frontend) ]
        │  HTTP Request
        ▼
[ API / Router ]          <- HTTP 통신 관문, 엔드포인트 라우팅, 상태 코드 반환
        │
        ▼
[ Schema (DTO) ]          <- Pydantic 기반 입력 데이터 검증 및 응답 구조 정의
        │
        ▼
[ Service ]               <- 순수 비즈니스 로직, AI/LLM 연동, 업무 규칙 제어
        │
        ▼
[ Repository ]            <- DB CRUD 쿼리 수행 (SQLAlchemy / SQL)
        │
        ▼
[ Model (Entity) ]        <- 데이터베이스 테이블 매핑 클래스
        │
        ▼
[ Database (PostgreSQL) ]
```

---

## 2. 계층별 역할 및 표준 디렉터리 구조

```text
backend/
└── app/
    ├── main.py              # 애플리케이션 진입점 (FastAPI 인스턴스 생성)
    ├── core/                # 전역 설정, 예외, 로깅 (config, exceptions, logging)
    ├── api/                 # 라우터 계층 (HTTP 요청/응답 처리)
    ├── schemas/             # DTO 스키마 계층 (Pydantic 모델)
    ├── services/            # 비즈니스 로직 및 AI 연동 계층
    ├── repositories/        # DB 접근 계층
    └── models/              # DB 테이블 ORM 엔티티 계층
```

### 계층별 책임 명세

| 계층 | 사용 기술 예시 | 주요 책임 | 주의사항 (Anti-Pattern) |
| :--- | :--- | :--- | :--- |
| **API / Router** | FastAPI Router | URL 매핑, 요청 파라미터 수신, HTTP 상태 코드 응답 | 라우터 내부에서 직접 SQL 실행 또는 LLM 호출 금지 |
| **Schema** | Pydantic BaseModel | 데이터 유입 유효성 검사, 필드 타입 캐스팅, 응답 직렬화 | DB 세부 구현이나 SQL 로직 포함 금지 |
| **Service** | Pure Python | 핵심 비즈니스 로직, 도메인 규칙, 외부 AI API(LLM) 연동 | HTTP 요청 객체(`Request`)에 직접 의존 금지 |
| **Repository** | SQLAlchemy / SQL | 데이터베이스 테이블과의 입출력(CRUD)만 전담 | 비즈니스 로직이나 의사결정 코드 포함 금지 |

---

## 3. 계층 간 협력 코드 예시

### 1) Schema (`app/schemas/document.py`)
```python
from pydantic import BaseModel, Field

class DocumentCreate(BaseModel):
    title: str = Field(..., min_length=1, max_length=100)
    content: str
```

### 2) Service (`app/services/document.py`)
```python
from app.schemas.document import DocumentCreate

class DocumentService:
    def summarize_and_save(self, data: DocumentCreate) -> str:
        # LLM 외부 호출 및 비즈니스 로직 수행
        summary = f"요약본: {data.title}"
        # 이후 Repository를 호출해 DB 저장 위임
        return summary
```

### 3) Router (`app/api/document.py`)
```python
from fastapi import APIRouter, status
from app.schemas.document import DocumentCreate
from app.services.document import DocumentService

router = APIRouter(prefix="/documents", tags=["documents"])
service = DocumentService()

@router.post("/", status_code=status.HTTP_201_CREATED)
def create_document(payload: DocumentCreate):
    result = service.summarize_and_save(payload)
    return {"status": "success", "result": result}
```