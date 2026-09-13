# Python 클래스 (Classes & OOP)

객체 지향 프로그래밍(OOP)의 핵심인 클래스 설계, 속성 스코프, 메서드 종류, 상속 및 추상화 문법 정리.

---

## 1. 클래스와 생성자 기본 구조

클래스는 객체(인스턴스)를 생성하기 위한 설계도이며, `__init__` 메서드를 통해 인스턴스 생성 시 속성을 초기화한다.

```python
class Person:
    # 생성자: 인스턴스 생성 시 자동 호출
    def __init__(self, name: str, age: int) -> None:
        self.name = name  # 인스턴스 변수
        self.age = age

    # 인스턴스 메서드
    def introduce(self) -> None:
        print(f"이름: {self.name}, 나이: {self.age}")

p1 = Person("Alice", 25)
p1.introduce()
```

---

## 2. 변수(속성)의 종류와 Scope

| 변수 종류 | 선언 위치 | 접근 방식 | 생명 주기 | 주요 특징 |
| :--- | :--- | :--- | :--- | :--- |
| **클래스 변수** | 클래스 블록 내부 | `클래스명.변수명` | 클래스 정의 ~ 프로그램 종료 | 모든 인스턴스가 공용으로 참조 |
| **인스턴스 변수** | 메서드 내부 (`self.변수명`) | `인스턴스.변수명` | 인스턴스 생성 ~ 인스턴스 소멸 | 객체마다 독립적인 메모리 공간 할당 |
| **지역 변수** | 메서드 내부 (`변수명`) | 메서드 내부에서만 | 메서드 호출 ~ 메서드 종료 | 외부 및 인스턴스에서 접근 불가 |

> **주의 (변수 섀도잉)**: `인스턴스.클래스변수명 = 값` 형태로 할당하면 공용 클래스 변수가 수정되는 것이 아니라, 동일한 이름의 인스턴스 변수가 생성되어 클래스 변수를 가린다.

---

## 3. 메서드의 종류

| 종류 | 데코레이터 | 첫 번째 매개변수 | 주요 용도 |
| :--- | :---: | :---: | :--- |
| **인스턴스 메서드** | 없음 | `self` | 인스턴스 속성 조회 및 변경 등 일반적인 작업 |
| **클래스 메서드** | `@classmethod` | `cls` | 클래스 속성 접근, 팩토리 메서드 구현 |
| **정적 메서드** | `@staticmethod` | 없음 | 클래스나 인스턴스 상태와 무관한 독립적 유틸리티 함수 |

```python
class Utility:
    base_rate = 1.1  # 클래스 변수

    @classmethod
    def update_rate(cls, new_rate: float) -> None:
        cls.base_rate = new_rate

    @staticmethod
    def add(a: int, b: int) -> int:
        return a + b
```

---

## 4. 특수 메서드 (Magic/Dunder Methods)

파이썬의 내장 연산자나 함수와 상호작용할 수 있도록 클래스 동작을 오버로딩한다.

| 메서드 | 트리거 시점 | 주요 역할 |
| :--- | :--- | :--- |
| `__str__(self)` | `print(obj)`, `str(obj)` | 사용자가 읽기 편한 문자열 반환 |
| `__repr__(self)` | 인터프리터 출력, `repr(obj)` | 객체의 공식적인 표현(디버깅용) 반환 |
| `__len__(self)` | `len(obj)` | 객체의 길이 반환 |
| `__eq__(self, other)` | `obj == other` | 동등성 비교 연산자 커스터마이징 |

---

## 5. 상속과 추상화

### 5.1 상속과 super()
부모 클래스의 속성과 메서드를 물려받으며, `super()`를 통해 부모의 초기화 로직을 재사용한다.

```python
class Animal:
    def __init__(self, name: str) -> None:
        self.name = name

class Dog(Animal):
    def __init__(self, name: str, breed: str) -> None:
        super().__init__(name)  # 부모 생성자 호출
        self.breed = breed
```

### 5.2 추상 클래스 (ABC)
`abc.ABC`와 `@abstractmethod`를 사용하여 자식 클래스가 특정 메서드를 반드시 구현하도록 강제한다.

```python
from abc import ABC, abstractmethod

class BaseProcessor(ABC):
    @abstractmethod
    def process(self) -> None:
        """자식 클래스에서 반드시 오버라이딩해야 함"""
        pass

class FileProcessor(BaseProcessor):
    def process(self) -> None:
        print("파일 처리 완료")
```