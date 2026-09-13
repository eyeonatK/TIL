# Python 예외 처리 (Exception Handling)

런타임 오류로 인한 프로그램의 비정상 종료를 방지하고, 안정적인 제어 흐름을 유지하기 위한 예외 처리 문법 정리.

---

## 1. 예외 처리 기본 구조

파이썬은 `try-except-else-finally` 블록을 통해 예외 상황을 세부적으로 제어한다.

```python
try:
    # 예외가 발생할 가능성이 있는 코드
    result = 10 / num
except ZeroDivisionError as e:
    # 특정 예외가 발생했을 때 실행
    print(f"0으로 나눌 수 없습니다: {e}")
except Exception as e:
    # 상위 예외 처리 (예측하지 못한 기타 오류)
    print(f"예기치 못한 오류 발생: {e}")
else:
    # 예외가 전혀 발생하지 않고 정상 종료되었을 때만 실행
    print(f"연산 성공: {result}")
finally:
    # 예외 발생 여부와 무관하게 항상 마지막에 실행 (자원 해제 등)
    print("연산 작업 완료")
```

---

## 2. 자주 발생하는 내장 예외 클래스

| 예외 클래스 | 발생 상황 | 코드 예시 |
| :--- | :--- | :--- |
| `ValueError` | 자료형은 맞으나 적절하지 않은 값 전달 | `int("abc")` |
| `TypeError` | 연산이나 함수에 잘못된 타입 전달 | `"str" + 1` |
| `KeyError` | 딕셔너리에 존재하지 않는 Key 조회 시도 | `{"a": 1}["b"]` |
| `IndexError` | 시퀀스의 인덱스 범위를 초과한 접근 | `[1, 2][5]` |
| `AttributeError` | 객체에 없는 속성이나 메서드 호출 | `None.name` |
| `FileNotFoundError` | 존재하지 않는 파일을 읽기 모드로 오픈 | `open("none.txt", "r")` |
| `ZeroDivisionError` | 숫자를 0으로 나누려고 시도 | `10 / 0` |

---

## 3. 예외 강제 발생 (`raise`)

비즈니스 로직에 부합하지 않는 상태가 발생했을 때 `raise` 키워드를 통해 의도적으로 예외를 던진다.

```python
def validate_age(age: int) -> None:
    if age < 0:
        raise ValueError("나이는 음수가 될 수 없습니다.")
```

---

## 4. 사용자 정의 예외 (Custom Exception)

내장 `Exception` 클래스를 상속받아 프로젝트의 도메인 상황에 특화된 커스텀 예외를 정의한다.

```python
# 사용자 정의 예외 선언
class InsufficientBalanceError(Exception):
    """잔액 부족 시 발생하는 예외"""
    pass

# 예외 활용
def withdraw(balance: int, amount: int) -> int:
    if amount > balance:
        raise InsufficientBalanceError(f"잔액 부족: 요청({amount}원) > 잔액({balance}원)")
    return balance - amount

try:
    withdraw(10000, 15000)
except InsufficientBalanceError as e:
    print(f"계좌 이체 실패: {e}")
```