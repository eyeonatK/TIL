# Python 제어문 (Control Flow)

파이썬의 실행 흐름을 제어하는 조건문과 반복문의 기본 문법 및 핵심 제어 흐름 정리.

---

## 1. 조건문 (Conditional Statements)

주어진 조건식의 참(True)/거짓(False) 여부에 따라 실행 경로를 분기한다.

### 문법 구조
- `elif`는 다중 조건을 분기할 때 사용하며, 여러 개 선언할 수 있다.
- `else`는 필수 요소가 아니며, 선행된 모든 조건이 거짓일 때 기본 블록으로 동작한다.
- 블록 단위 구분은 들여쓰기(Indentation, 4칸 공백 권장)를 엄격히 준수해야 한다.

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

### 2.1 While 문
조건식이 `True`를 유지하는 동안 내부 블록을 반복한다. 탈출 조건을 갱신하지 않으면 무한 루프(Infinite Loop)가 발생하므로 주의해야 한다.

```python
count = 0

while count < 3:
    print(f"현재 카운트: {count}")
    count += 1
```

### 2.2 반복 제어 키워드

| 제어문 | 기능 설명 |
| :--- | :--- |
| `break` | 현재 실행 중인 반복문을 즉시 중단하고 루프 블록을 완전히 탈출 |
| `continue` | 현재 루프 회차의 남은 실행 코드를 건너뛰고 다음 반복 회차로 진입 |

```python
num = 1
result = 0

while num <= 5:
    if num % 2 == 0:
        num += 1
        continue
    result += num
    num += 1

# result: 9 (1 + 3 + 5)
```

---

## 3. 실습 코드 정리

```python
# 1. 0부터 15까지 출력
i = 0
while i <= 15:
    print(i, end=" ")
    i += 1

# 2. 0부터 100까지 10 단위로 출력
i = 0
while i <= 100:
    print(i, end=" ")
    i += 10

# 3. 1부터 50까지 중 홀수만 출력
i = 1
while i <= 50:
    if i % 2 != 0:
        print(i, end=" ")
    i += 1

# 4. 1부터 50까지의 총합 계산
i = 1
total = 0
while i <= 50:
    total += i
    i += 1
print(f"1~50 합: {total}")

# 5. 1부터 100까지 짝수들의 총합 계산
i = 1
even_total = 0
while i <= 100:
    if i % 2 == 0:
        even_total += i
    i += 1
print(f"1~100 짝수 합: {even_total}")
```