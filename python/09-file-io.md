# Python 파일 입출력 (File I/O)

파일 핸들링 기본 모드, `with` 컨텍스트 매니저를 통한 안전한 자원 해제, 그리고 `pathlib`을 이용한 크로스 플랫폼 경로 처리 정리.

---

## 1. 파일 열기 모드 (File Modes)

`open(파일경로, 모드, encoding="utf-8")` 함수를 사용하여 파일을 연다. 한글 깨짐 방지를 위해 `encoding="utf-8"` 지정을 권장한다.

| 모드 | 설명 | 대상 파일 존재 시 | 대상 파일 미존재 시 |
| :---: | :--- | :--- | :--- |
| `'r'` | **읽기 (Read)** | 파일 정상 오픈 | `FileNotFoundError` 발생 |
| `'w'` | **쓰기 (Write)** | **기존 내용 삭제 후 덮어쓰기** | 새로운 파일 생성 |
| `'a'` | **추가 (Append)** | 기존 내용 유지하며 끝에 추가 | 새로운 파일 생성 |
| `'x'` | **배타적 생성** | `FileExistsError` 발생 | 새로운 파일 생성 |
| `'b'` | **바이너리 모드** | 이미지, 바이너리 데이터 입출력 (예: `'rb'`, `'wb'`) | - |

---

## 2. With 문을 활용한 안전한 파일 처리

`open()`을 단독으로 사용하면 작업 완료 후 `f.close()`를 직접 호출해야 한다. 만약 중간에 에러가 나면 파일이 닫히지 않고 누수가 발생할 수 있으므로, 블록 종료 시 자동으로 `close()`를 호출해 주는 `with` 문 사용이 기본 원칙이다.

```python
# 1. 파일 쓰기 (기존 내용 덮어쓰기)
with open("sample.txt", "w", encoding="utf-8") as f:
    f.write("첫 번째 줄입니다.\n")
    f.write("두 번째 줄입니다.\n")

# 2. 파일 이어 쓰기 (Append)
with open("sample.txt", "a", encoding="utf-8") as f:
    f.write("추가된 줄입니다.\n")
```

---

## 3. 파일 읽기 방식

```python
# 방식 1: 파일 전체 내용을 하나의 문자열로 읽기
with open("sample.txt", "r", encoding="utf-8") as f:
    content = f.read()

# 방식 2: 전체 줄을 읽어 리스트(list[str])로 반환
with open("sample.txt", "r", encoding="utf-8") as f:
    lines = f.readlines()

# 방식 3: 대용량 파일용 제너레이터 순회 (메모리 효율적, 권장)
with open("sample.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())  # strip()으로 개행문자(\n) 제거
```

---

## 4. 경로 처리 (`pathlib.Path`)

OS마다 다른 경로 구분자(Windows `\`, Unix/macOS `/`) 문제와 이스케이프 문자 오류를 방지하기 위해 `pathlib.Path`를 사용한다.

```python
from pathlib import Path

# 현재 실행 파일 기준 경로 탐색
current_file = Path(__file__).resolve()
current_dir = current_file.parent
project_root = current_dir.parent

# / 연산자를 사용한 안전한 경로 결합
data_dir = project_root / "data"
target_file = data_dir / "result.txt"

# 파일 및 디렉터리 존재 여부 확인
if target_file.exists():
    print("파일이 이미 존재합니다.")
```