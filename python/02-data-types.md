# Python 변수와 자료형 (Data Types)

파이썬의 동적 타이핑 특성과 기본 자료형, 그리고 복합 자료형(컬렉션)의 핵심 문법 및 메서드 정리.

---

## 1. 동적 타이핑 (Dynamic Typing)

파이썬은 변수 선언 시 타입을 명시하지 않고, 대입되는 값에 따라 인터프리터가 타입을 자동으로 결정한다.

| 언어 구분 | 특징 | 대표 언어 |
| :--- | :--- | :--- |
| **정적 타입 (Static Type)** | 컴파일 시점에 타입을 지정 및 고정 | C, C++, Java |
| **동적 타입 (Dynamic Type)** | 런타임에 값에 따라 타입이 자동 결정 | Python, JavaScript |

---

## 2. 기본 자료형 (Scalar Types)

단일 값을 표현하는 내장 자료형.

| 자료형 | 타입명 | 예시 | 특징 |
| :--- | :--- | :--- | :--- |
| 정수형 | `int` | `10`, `-3`, `0` | 크기 제한 없음 |
| 실수형 | `float` | `3.14`, `-0.5` | 부동소수점 방식 |
| 문자열 | `str` | `"hello"`, `'python'` | 불변(Immutable), 인덱싱/슬라이싱 가능 |
| 불리언 | `bool` | `True`, `False` | 조건 판단의 기준 |
| 널 타입 | `NoneType` | `None` | 값이 없음을 명시 |

---

## 3. 복합 자료형 (Collections)

여러 개의 데이터를 구조화하여 관리하는 컬렉션 자료구조.

| 구분 | 순서 (Index) | 수정 가능 (Mutable) | 중복 허용 | 기본 문법 |
| :--- | :---: | :---: | :---: | :--- |
| `list` | O | O | O | `[1, 2, 3]` |
| `tuple` | O | X | O | `(1, 2, 3)` |
| `dict` | O (3.7+) | O (Key는 불변) | Key 불가, Value 허용 | `{"key": "val"}` |
| `set` | X | O | X | `{1, 2, 3}` |

### 3.1 List (리스트)
가장 널리 쓰이는 가변 시퀀스 자료형.

```python
data = [1, 2, 3]

data.append(4)         # 맨 뒤에 추가
data.insert(1, 100)    # 특정 인덱스에 삽입
data.remove(2)         # 특정 값 삭제
popped = data.pop()    # 마지막 값 제거 및 반환
data.sort()            # 오름차순 정렬
```

### 3.2 Tuple (튜플)
읽기 전용 데이터를 관리할 때 사용하며, 리스트 대비 메모리 효율이 높다.

- **주의**: 단일 원소 튜플 선언 시 반드시 쉼표(`,`)를 붙여야 한다.
  - `a = (10)` -> `int`
  - `a = (10,)` -> `tuple`

### 3.3 Dict (딕셔너리)
Key-Value 쌍으로 데이터를 매핑하며 해시 테이블 기반으로 빠른 조회가 가능하다.

```python
user = {"name": "Alice", "age": 25}

name = user.get("name")      # 권장: 키가 없어도 KeyError 방지 (None 반환)
keys = user.keys()           # 키 목록
values = user.values()       # 값 목록
items = user.items()         # (키, 값) 튜플 쌍 목록
```

### 3.4 Set (집합)
순서가 없으며 고유한 값만 유지하므로 중복 필터링에 효과적이다.

```python
raw = [1, 2, 2, 3, 3, 3]
unique_set = set(raw)  # {1, 2, 3} (len: 3)

unique_set.add(4)
unique_set.discard(2)   # 값이 없어도 에러를 내지 않고 안전하게 제거
```