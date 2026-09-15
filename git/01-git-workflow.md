# Git & GitHub 기초 및 프로젝트 워크플로우

로컬 저장소 생성, 스테이징, 커밋, 원격 저장소 동기화(`push`) 및 `.gitignore`를 통한 민감 정보 관리 정리.

---

## 1. Git 핵심 영역 구조 (3-Tier Architecture)

Git은 변경 사항을 세 단계로 나누어 안전하게 스냅샷을 기록한다.

```text
[ Working Directory ]  --( git add )-->  [ Staging Area (Index) ]  --( git commit )-->  [ Local Repository ]
    (작업 공간)                                (커밋 대기 공간)                               (로컬 저장소 스냅샷)
```

- **Working Directory**: 개발자가 코드를 작성하고 수정하는 실제 작업 공간.
- **Staging Area**: 다음 커밋에 반영할 파일들을 선별해 올려두는 임시 저장소.
- **Local Repository**: `.git` 디렉터리에 커밋 단위로 영구 기록되는 로컬 이력 저장소.

---

## 2. 초기 사용자 환경 설정 (CLI)

Git 설치 후 커밋 작성자를 식별하기 위해 전역(Global) 설정을 1회 수행한다.

```bash
# 사용자 이름 및 이메일 설정
git config --global user.name "사용자명"
git config --global user.email "이메일@example.com"

# 설정 확인
git config --global --list
```

---

## 3. 핵심 명령어 치트시트

| 단계 | 명령어 | 역할 및 설명 |
| :--- | :--- | :--- |
| **저장소 초기화** | `git init` | 현재 디렉터리를 Git 로컬 저장소로 지정 (`.git` 생성) |
| **상태 확인** | `git status` | 변경된 파일, 스테이징 여부, Untracked 파일 목록 확인 |
| **스테이징** | `git add .` | 수정된 모든 변경 사항을 Staging Area로 등록 |
| **커밋 생성** | `git commit -m "메시지"` | Staging Area의 내용을 로컬 스냅샷으로 영구 저장 |
| **원격지 연결** | `git remote add origin <URL>` | GitHub 원격 저장소 주소에 `origin`이라는 별칭 부여 |
| **원격지 확인** | `git remote -v` | 현재 연결된 원격 저장소 URL 확인 |
| **원격지 푸시** | `git push -u origin main` | 로컬 `main` 브랜치의 커밋 내역을 GitHub에 업로드 |

---

## 4. `.gitignore` 설정 및 주의사항

API 키, 가상환경, 개인 설정 등 버전 관리에 포함되지 않아야 할 파일들을 정의한다.

### 권장 기본 설정
```text
# 가상환경
.venv/
venv/

# 환경 변수 (민감 정보 노출 방지)
.env

# 파이썬 캐시
__pycache__/
*.py[cod]
.pytest_cache/

# 에디터 설정
.vscode/
.idea/

# 로그 파일
*.log
```

> **주의**: 이미 `git add` 되어 로컬 저장소에 추적(Tracked)된 파일은 나중에 `.gitignore`에 등록해도 계속 추적된다. 따라서 **첫 커밋(`git add .`)을 실행하기 전에 반드시 `.gitignore`를 먼저 생성**해야 한다.