# pytest 테스트 자동화 기초

테스트 프레임워크인 pytest의 자동 탐색 규칙, `assert`를 통한 검증, 그리고 `pytest.raises`를 활용한 예외 검증 기법 정리.

---

## 1. pytest 핵심 원리

별도의 복잡한 설정이나 등록 절차 없이, **정해진 이름 규칙(Convention)**에 따라 작성된 파일과 함수를 자동으로 찾아내어(Test Discovery) 실행한다.

```bash
# pytest 설치
python -m pip install pytest
```

---

## 2. 네이밍 규칙 (Discovery Convention)

| 구분 | 규칙 | 예시 |
| :--- | :--- | :--- |
| **테스트 파일** | `test_*.py` 또는 `*_test.py` | `test_calculator.py` |
| **테스트 함수** | `test_`로 시작 | `def test_add_success():` |
| **테스트 클래스** | `Test*`로 시작 (`__init__` 미사용) | `class TestDocumentService:` |

---

## 3. 기본 검증 (`assert`)

파이썬 기본 문법인 `assert`를 사용하여 기대값과 실제 결과값의 일치 여부를 판단한다.

```python
# calculator.py
def add(a: int, b: int) -> int:
    return a + b

# test_calculator.py
from calculator import add

def test_add():
    assert add(10, 20) == 30      # 기대값 30과 일치 -> PASS
    assert add(-1, 1) == 0        # PASS
```

---

## 4. 예외 발생 검증 (`pytest.raises`)

특정 상황에서 의도한 예외가 제대로 발생하는지 확인할 때는 `pytest.raises(예외클래스)` 컨텍스트 매니저를 사용한다.

```python
import pytest

def divide(a: int, b: int) -> float:
    if b == 0:
        raise ZeroDivisionError("0으로 나눌 수 없습니다.")
    return a / b

def test_divide_by_zero():
    # 블록 내부에서 ZeroDivisionError가 발생해야 테스트 통과
    with pytest.raises(ZeroDivisionError) as exc_info:
        divide(10, 0)

    # 에러 메시지 내용까지 세부 검증 가능
    assert "0으로 나눌 수 없습니다" in str(exc_info.value)
```

---

## 5. 주요 CLI 실행 옵션

터미널에서 테스트를 실행하고 디버깅할 때 자주 사용하는 플래그 모음:

```bash
# 1. 전체 테스트 실행
pytest

# 2. 상세 결과 출력 (-v, Verbose)
pytest -v

# 3. 테스트 내부의 print() 출력 콘솔에 표시 (-s)
pytest -s

# 4. 특정 키워드가 포함된 함수/파일만 선별 실행 (-k)
pytest -k "divide"

# 5. 첫 번째 실패 발생 시 즉시 중단 (-x)
pytest -x
```