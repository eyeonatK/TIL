# Python 개발 환경 설정 (Environment Setup)

Windows 환경에서의 Python 3.12 설치, VSCode 설정 및 venv 가상환경 구성 가이드.

---

## 1. Python 설치 시 핵심 체크포인트

설치 마법사 실행 시 아래 옵션을 반드시 확인한다.

* **Add python.exe to PATH**: 시스템 환경 변수에 등록하여 어느 경로에서든 CLI로 파이썬을 호출할 수 있도록 체크
* **Use admin privileges when installing py.exe**: 권한 문제 방지를 위해 체크 권장

설치 확인 명령어:
```bash
python --version
# 또는
py --version
```

---

## 2. VSCode 기본 세팅

1. **필수 확장 프로그램 (Extensions)**
   * `Python` (Microsoft)
   * `Jupyter`
2. **기본 터미널 변경**
   * PowerShell의 보안 정책(ExecutionPolicy) 에러를 방지하기 위해 기본 터미널 프로필을 **Command Prompt (CMD)**로 설정 권장

---

## 3. venv 가상환경 구축 치트시트

프로젝트별 패키지 의존성 격리를 위해 작업 디렉터리 내에 독립된 가상환경을 생성 및 사용한다.

### 가상환경 생명주기 명령어

| 동작 | Windows (CMD) | macOS / Linux |
| :--- | :--- | :--- |
| **가상환경 생성** | `python -m venv venv` | `python3 -m venv venv` |
| **가상환경 활성화** | `venv\Scripts\activate.bat` | `source venv/bin/activate` |
| **가상환경 비활성화** | `deactivate` | `deactivate` |

* 활성화 성공 시 터미널 프롬프트 앞에 `(venv)` 접두어가 표시된다.

---

## 4. VSCode 인터프리터 연동

가상환경을 생성한 후 VSCode가 해당 격리 환경을 바라보도록 설정해야 한다.

1. 단축키 `Ctrl + Shift + P` 입력
2. `Python: Select Interpreter` 검색 및 실행
3. 목록에서 `./venv/Scripts/python.exe` (Workspace) 선택