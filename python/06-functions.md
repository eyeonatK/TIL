# Python 함수 (Functions)

작업 단위의 모듈화 및 재사용을 위한 함수 정의 문법, 매개변수 패킹(`*args`, `**kwargs`), 일급 객체 특성 정리.

---

## 1. 함수의 정의 및 타입 힌트

함수는 `def` 키워드로 정의하며, 매개변수와 반환값의 타입을 명시할 수 있다.

```python
def add(a: int, b: int) -> int:
    return a + b

# 반환값이 없는 경우 None 표기
def print_message(msg: str) -> None:
    print(msg)
```

---

## 2. 매개변수(Parameter) 전달 방식

### 2.1 기본값 (Default Argument)
호출 시 인자를 생략하면 지정된 기본값이 사용된다. 기본값 매개변수는 일반 매개변수보다 항상 뒤에 배치해야 한다.

```python
def greet(name: str, age: int = 20) -> None:
    print(f"{age}세 {name}님 환영합니다.")

greet("Alice")       # 20세 Alice님 환영합니다.
greet("Bob", 30)     # 30세 Bob님 환영합니다.
```

### 2.2 가변 인자: `*args` (Positional Packing)
임의 개수의 위치 인자를 하나의 **튜플(tuple)** 형태로 묶어서 받는다.

```python
def sum_all(*nums: int) -> int:
    # nums는 전달된 모든 위치 인자를 담은 튜플
    return sum(nums)

print(sum_all(10, 20, 30))  # 60
```

### 2.3 가변 키워드 인자: `**kwargs` (Keyword Packing)
`key=value` 형태로 전달되는 임의 개수의 인자를 하나의 **딕셔너리(dict)** 형태로 묶어서 받는다.

```python
def configure_system(**kwargs: str) -> None:
    # kwargs는 {"env": "prod", "debug": "false"} 형태의 딕셔너리
    for key, value in kwargs.items():
        print(f"{key}: {value}")

configure_system(env="prod", debug="false")
```

---

## 3. 함수의 고급 특성

### 3.1 다중 반환값 (Multiple Return Values)
콤마(`,`)로 구분하여 여러 값을 반환할 수 있으며, 내부적으로는 **단일 튜플**로 묶여 반환된 후 언패킹된다.

```python
def get_user_info() -> tuple[str, int]:
    return "Alice", 25

name, age = get_user_info()
```

### 3.2 일급 객체 (First-Class Object)
파이썬의 함수는 객체이므로 변수에 할당하거나, 다른 함수의 인자로 전달하거나, 함수에서 함수를 반환할 수 있다.

```python
def say_hello() -> None:
    print("Hello, World!")

def execute(func) -> None:
    func()

execute(say_hello)  # 함수 자체를 인자로 전달
```

---

## 4. 실습 코드 정리

```python
# 1. 인덱스 2개를 입력받아 요소 교환
def swap_elements(lst: list, idx1: int, idx2: int) -> None:
    lst[idx1], lst[idx2] = lst[idx2], lst[idx1]

# 2. 리스트 역순 정렬
numbers = [1, 3, 4, 5, 2]
numbers.sort(reverse=True)  # [5, 4, 3, 2, 1]

# 3. 중첩 리스트 인덱싱
lst = ["korea", ["IT", [1, 3, 5, 7, 9], ["even", [0, 2, 4, 6, 8]]]]
print(lst[0])            # korea
print(lst[1][1][1])      # 3
print(lst[1][2][1][4])   # 8

# 4. 3과목 점수 총점 및 평균 계산
def calc_score(kor: int, eng: int, math: int) -> tuple[int, float]:
    total = kor + eng + math
    avg = total / 3
    return total, avg

# 5. 간단한 카페 주문 합산 예시
def run_cafe() -> None:
    menu = {1: 2000, 2: 3000, 3: 4000, 4: 4500}
    total_price = 0
    while True:
        choice = int(input("메뉴 선택 (1~4, 종료는 5): "))
        if choice == 5:
            break
        if choice in menu:
            total_price += menu[choice]
    print(f"총 주문 금액: {total_price}원")
```