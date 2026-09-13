# Python 모듈과 패키지 (Modules & Packages)

코드의 모듈화와 재사용성을 위한 모듈(`.py`), 패키지(디렉터리), 그리고 다양한 임포트(import) 방식 정리.

---

## 1. 모듈 (Module)

파이썬 함수, 클래스, 변수를 정의해 둔 단일 파일(`.py`) 단위.

### 모듈 작성 및 호출 예시
```python
# math_utils.py (모듈 정의)
def add(a: int, b: int) -> int:
    return a + b

def subtract(a: int, b: int) -> int:
    return a - b
```

```python
# main.py (모듈 사용)
import math_utils

result = math_utils.add(10, 20)
print(result)  # 30
```

---

## 2. 모듈 Import 방식 비교

| 방식 | 문법 예시 | 설명 및 사용 시점 |
| :--- | :--- | :--- |
| **모듈 전체** | `import math_utils` | 모듈명을 네임스페이스로 사용하여 명확한 구분이 필요할 때 |
| **별칭 지정** | `import math_utils as mu` | 모듈명이 길어 타이핑을 줄이고자 할 때 |
| **특정 요소만** | `from math_utils import add` | 특정 함수나 클래스만 직접 호출하여 가독성을 높일 때 |
| **전체 유입 (비권장)** | `from math_utils import *` | 이름 충돌(Namespace Pollution) 및 출처 파악이 어려워 지양 |

---

## 3. 패키지 (Package)

여러 모듈을 폴더(디렉터리) 구조로 묶어 계층적으로 관리하는 단위.

### 패키지 디렉터리 아키텍처
```text
my_project/
├── main.py
└── my_package/
    ├── __init__.py       # 패키지 인식 및 초기화 파일 (내용 비움 가능)
    ├── calculator.py     # 모듈 1
    └── converter.py      # 모듈 2
```

### 임포트 경로 표기법
* **절대 경로 (권장)**: 프로젝트 루트 디렉터리를 기준으로 지정
  ```python
  from my_package.calculator import add
  from my_package import converter
  ```
* **상대 경로**: 현재 작업 파일 위치를 기준으로 점(`.`)을 찍어 지정
  ```python
  from . import calculator      # 현재 폴더
  from ..utils import helper    # 상위 폴더의 utils
  ```
> **주의**: 상대 경로는 패키지 내부에서 상호 참조할 때 주로 사용하며, 해당 모듈을 단독(`python calculator.py`)으로 직접 실행할 경우 `ImportError`가 발생할 수 있다.