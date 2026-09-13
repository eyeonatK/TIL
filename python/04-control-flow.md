# Python 제어문 (Control Flow)

파이썬의 실행 흐름을 제어하는 조건문과 반복문(`while`, `for`), 그리고 시퀀스 처리 문법 정리.

---

## 1. 조건문 (Conditional Statements)

주어진 조건식의 참(True)/거짓(False) 여부에 따라 실행 경로를 분기한다.

### 문법 구조
- `elif`는 다중 조건을 분기할 때 사용하며, 여러 개 선언할 수 있다.
- `else`는 필수 요소가 아니며, 선행된 모든 조건이 거짓일 때 실행된다.
- 블록 단위 구분은 들여쓰기(Indentation, 공백 4칸)를 엄격히 준수한다.

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"
```

---

## 2. 반복문 (Loops)

특정 조건을 만족하거나 정의된 시퀀스를 순회할 때 코드를 반복 실행한다.

| 반복문 | 주요 용도 |
| :--- | :--- |
| `while` | 조건식이 참(`True`)인 동안 무기한 혹은 조건 충족 시까지 반복 |
| `for` | 리스트, 튜플, 문자열 등 순회 가능한 객체(Iterable)의 요소를 차례로 탐색 |

### 2.1 While 문
조건식이 `True`인 동안 반복 실행되므로, 루프 내부에서 탈출 조건을 갱신해야 한다.

```python
count = 0
while count < 3:
    print(f"현재 카운트: {count}")
    count += 1
```

### 2.2 For 문 (for ... in)
시퀀스나 이터러블 객체의 원소를 하나씩 변수에 바인딩하며 순회한다.

```python
# range(시작, 끝, 간격) -> 끝-1까지 순회
for i in range(1, 6, 2):
    print(i)  # 1, 3, 5 출력
```

### 2.3 enumerate() 함수
반복문 순회 시 요소의 값뿐만 아니라 **(인덱스, 값)** 형태의 튜플 언패킹을 제공한다.

```python
fruits = ["apple", "banana", "kiwi"]

for idx, fruit in enumerate(fruits):
    print(f"{idx}번 과일: {fruit}")
```

### 2.4 반복 제어 키워드

| 제어문 | 기능 설명 |
| :--- | :--- |
| `break` | 현재 실행 중인 반복문을 즉시 중단하고 루프 블록을 완전히 탈출 |
| `continue` | 현재 루프 회차의 남은 실행 코드를 건너뛰고 다음 반복 회차로 진입 |

---

## 3. 리스트 컴프리헨션 (List Comprehension)

반복문과 조건식을 한 줄로 압축하여 새로운 리스트를 간결하게 생성하는 문법.

### 기본 구조
```python
[표현식 for 변수 in 반복가능한객체]
[표현식 for 변수 in 반복가능한객체 if 조건식]
```

### 활용 예시
```python
words = ["apple", "banana", "kiwi", "melon"]

# 길이가 4 초과인 문자열만 인덱스와 함께 대문자로 변환
result = [f"{idx}_{w.upper()}" for idx, w in enumerate(words) if len(w) > 4]
# result: ['0_APPLE', '1_BANANA', '3_MELON']
```

---

## 4. 실습 코드 정리

```python
# 1. 0부터 15까지 출력
for i in range(16):
    print(i, end=" ")

# 2. 0부터 100까지 10 단위로 출력
for i in range(0, 101, 10):
    print(i, end=" ")

# 3. 1부터 50까지 중 홀수만 출력
for i in range(1, 51, 2):
    print(i, end=" ")

# 4. 1부터 50까지의 총합 계산
total = sum(range(1, 51))
print(f"1~50 합: {total}")

# 5. 1부터 100까지 짝수들의 총합 계산
even_total = sum(i for i in range(1, 101) if i % 2 == 0)
print(f"1~100 짝수 합: {even_total}")
```