# Today I Learned

매일 학습한 기술적 개념과 트러블슈팅 내역을 정리하는 저장소입니다.

## Directory Index

### Git & Version Control
- [Git 기본 아키텍처 및 핵심 워크플로우](git/git-basic.md)
## 🐙 Git & GitHub

| 번호 | 주제 | 상세 내용 | 링크 |
| :--- | :--- | :--- | :--- |
| 01 | Git 워크플로우 | 3영역 구조, 기본 CLI, 원격 저장소 연동, `.gitignore` | [바로가기](git/01-git-workflow.md) |
---
## 🌐 Network & HTTP

| 번호 | 주제 | 상세 내용 | 링크 |
| :--- | :--- | :--- | :--- |
| 01 | 네트워크 및 HTTP 기초 | IP/Port, TCP 3-way handshake, UDP, DNS, URI 구조 | [바로가기](network/01-network-and-http-basics.md) |
| 02 | HTTP 메시지와 웹 API 통신 | HTTP 메시지 구조, Method, 상태 코드, Header | [바로가기](network/02-http-messages-and-methods.md) |
---
## ⚡ FastAPI

| 번호 | 주제 | 상세 내용 | 링크 |
| :--- | :--- | :--- | :--- |
| 01 | FastAPI 기초 및 요청 파라미터 | Uvicorn, Endpoint, Path/Query Parameter, Query 검증 | [바로가기](fastapi/01-fastapi-basics.md) |
---
## Python

| 번호 | 주제 | 상세 내용 | 링크 |
| :--- | :--- | :--- | :--- |
| 01 | 개발 환경 설정 | Python 3.12 설치, VSCode 가상환경(venv) 연동 | [바로가기](python/01-environment-setup.md) |
| 02 | 변수와 자료형 | 기본형, 시퀀스, 컬렉션(List, Tuple, Dict, Set) | [바로가기](python/02-data-types.md) |
| 03 | 연산자 | 산술, 비교, 논리, 삼항 연산자 | [바로가기](python/03-operators.md) |
| 04 | 제어문 | 조건문(`if`), 반복문(`while`), `break`/`continue` | [바로가기](python/04-control-flow.md) |
| 05 | 타입힌트 | 변수/컬렉션 어노테이션, Union(`\|`), Ellipsis(`...`) | [바로가기](python/05-type-hints.md) |
| 06 | 함수 | def, 매개변수, 가변인자 | [바로가기](python/06-functions.md) |
| 07 | 클래스 | 인스턴스/클래스 변수, 상속, 추상화 | [바로가기](python/07-classes.md) |
| 08 | 모듈과 패키지 | Import 문법, `__init__.py`, 패키지 구조 | [바로가기](python/08-modules-and-packages.md) |
| 09 | 파일 입출력 | 파일 핸들러 모드, `with` 컨텍스트 매니저, `pathlib.Path` | [바로가기](python/09-file-io.md) |
| 10 | 예외 처리 | `try-except-else-finally`, `raise`, 사용자 정의 예외 | [바로가기](python/10-exceptions.md) |
| 11 | Python Pydantic | BaseModel, Field 제약, 직렬화, Validator | [바로가기](python/11-pydantic.md) |
| 12 | 환경 변수 관리 | .env, Pydantic Settings, 싱글톤 패턴 | [바로가기](python/12-environment-variables.md) |
| 13 | 계층형 아키텍처 | Router-Service-Repository | [바로가기](python/13-layered-architecture.md) |
| 14 | 도메인 예외 계층 설계 | AgentError 상속, raise from, 에러 핸들링 | [바로가기](python/14-custom-exceptions.md) |
| 15 | 엔터프라이즈 로깅 | 로그 레벨, 단일 루트 로거, 노이즈 필터링 | [바로가기](python/15-logging.md) |
| 16 | 테스트 자동화 기초 | pytest, 이름 규칙, assert, pytest.raises | [바로가기](python/16-pytest-basics.md) |
