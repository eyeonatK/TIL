**Streamlit 기능을 처음부터 배우겠습니다.**

작은 장난감 가게를 만든다고 생각해 보세요.

가게에는 간판도 있고, 물건을 고르는 곳도 있고, 주문서를 쓰는 곳도 있죠. Streamlit은 이런 것들을 **파이썬 코드로 화면에 놓을 수 있게 해주는 도구**예요.

설명은 아주 쉽게 하되, 실제 동작은 정확하게 짚어볼게요. 아래 예제는 별도 설명이 없으면 맨 위에 `import streamlit as st`가 있다고 생각하면 됩니다.

---

**1. 먼저, 코드 한 줄을 읽는 방법**

```python
import streamlit as st
```

“Streamlit이라는 도구 상자를 가져오고, 앞으로는 짧게 `st`라고 부를게”라는 뜻이에요.

이제 도구를 꺼내 써봅시다.

```python
st.title("우리 가게")
```

나누어 읽으면 이렇습니다.

| 코드 | 뜻 |
|---|---|
| `st` | Streamlit 도구 상자 |
| `.` | 그 안에 있는 |
| `title` | 큰 제목을 만드는 도구 |
| `(...)` | 도구에 전달할 내용을 넣는 자리 |
| `"우리 가게"` | 화면에 쓸 글자 |

`title`처럼 정해진 일을 해주는 도구를 **함수**라고 불러요.

괄호에 넣는 재료는 **인자**라고 합니다. 처음에는 “도구에 주는 재료”라고 생각해도 좋아요.

```python
st.title("우리 가게")
st.write("어서 오세요!")
```

파이썬은 위에서 아래로 읽으므로, 제목 다음에 인사말이 나옵니다.

---

**2. 가장 중요한 원리: 선택하면 다시 읽어요**

우리가 만든 앱은 보통 이렇게 움직입니다.

```text
코드를 위에서 아래로 읽기
        ↓
화면 만들기
        ↓
사용자가 메뉴를 선택하거나 버튼 누르기
        ↓
새로운 입력값으로 코드를 다시 읽기
        ↓
화면 다시 만들기
```

이렇게 코드를 다시 실행하는 것을 **재실행**, 영어로는 *rerun*이라고 해요.

단, 뒤에서 배울 `form`은 여러 입력을 모았다가 제출할 때 전달하는 예외입니다. [공식 실행 흐름 설명](https://docs.streamlit.io/develop/api-reference/execution-flow)

이 원리를 알면 메뉴, 버튼, 저장 기능을 이해하기 쉬워집니다.

---

**3. `set_page_config`: 가게 건물의 기본 모습 정하기**

```python
st.set_page_config(
    page_title="우리 가게",
    page_icon="🛒",
    layout="wide",
)
```

| 설정 | 하는 일 |
|---|---|
| `page_title` | 브라우저 탭에 표시할 이름 |
| `page_icon` | 브라우저 탭에 표시할 작은 그림 |
| `layout="wide"` | 화면을 넓게 사용 |
| `layout="centered"` | 내용을 가운데에 모아 표시 |

여기서 꼭 구별하세요.

```python
st.set_page_config(page_title="우리 가게")
st.title("오늘의 주문")
```

브라우저 탭에는 **우리 가게**, 화면 안에는 **오늘의 주문**이 나와요.

우리 예제에서는 기본 설정을 알아보기 쉽게 코드 앞부분에 두었습니다.

---

**4. 글자를 보여주는 도구들**

가게에는 큰 간판, 작은 안내문, 가격표가 필요하겠죠.

| 함수 | 역할 | 비유 |
|---|---|---|
| `title` | 큰 제목 | 가게 간판 |
| `subheader` | 구역 제목 | “과자 코너” 표지판 |
| `caption` | 작고 은은한 설명 | 표지판 아래 작은 안내 |
| `write` | 글이나 데이터를 표시 | 여러 용도로 쓰는 게시판 |
| `markdown` | 서식이 있는 글 표시 | 글자를 꾸미는 게시판 |
| `code` | 코드를 코드 모양으로 표시 | 코드 견본 상자 |

```python
st.title("🛒 우리 가게")
st.caption("즐거운 장난감 가게입니다.")
st.subheader("오늘의 추천")
st.write("곰 인형을 추천해요.")
```

`write`는 글뿐 아니라 숫자나 목록도 보여줄 수 있어요.

```python
st.write(100)
st.write(["곰 인형", "자동차", "공"])
```

`markdown`은 특별한 기호로 글을 꾸밉니다.

```python
st.markdown("**아주 중요한 글**")
st.markdown("*살짝 강조한 글*")
st.markdown("- 사과\n- 바나나")
```

- `**글**`: 굵은 글씨
- `*글*`: 기울인 글씨
- `- 글`: 목록
- `\n`: 줄바꿈

`code`는 코드를 **보여주기만** 합니다.

```python
st.code('print("안녕!")', language="python")
```

이 코드는 화면에 `print("안녕!")`를 보여줘요. 안에 적힌 `print`를 실행하는 것은 아닙니다.

---

**5. `divider`: 구역 사이에 줄 긋기**

```python
st.write("위쪽 내용")
st.divider()
st.write("아래쪽 내용")
```

내용 사이에 가로선을 그어요.

공책에 줄을 그어 “여기부터 다른 이야기야”라고 구분하는 것과 같습니다.

---

**6. `columns`: 화면을 옆으로 나누기**

기본적으로 Streamlit은 내용을 위에서 아래로 놓아요.

```python
st.write("사과")
st.write("바나나")
```

결과:

```text
사과
바나나
```

옆으로 놓고 싶을 때 `columns`를 사용합니다.

```python
left, right = st.columns(2)

with left:
    st.write("사과")

with right:
    st.write("바나나")
```

결과:

```text
사과          바나나
```

`left`, `right`는 우리가 붙인 이름입니다. 꼭 이 이름이어야 하는 것은 아니에요.

너비도 나눌 수 있습니다.

```python
left, right = st.columns([3, 2])
```

전체를 다섯 조각으로 생각해서:

- 왼쪽은 세 조각
- 오른쪽은 두 조각

을 사용하는 비율입니다. 가운데 간격을 제외한 공간을 이 비율로 나눠요.

---

**7. `with`: “이 안에 넣어줘”**

앞의 예제에 이런 코드가 있었죠.

```python
with left:
    st.write("사과")
```

여기서 `with left:`는 **“아래 내용을 왼쪽 칸에 넣어줘”**라는 뜻으로 이해하면 됩니다.

들여쓰기도 중요해요.

```python
with left:
    st.write("사과")
    st.write("딸기")

st.write("여기는 칸 밖이에요")
```

앞에 공백이 들어간 두 줄만 왼쪽 칸에 들어갑니다.

`with` 자체는 Streamlit 함수가 아니라 **파이썬 문법**이에요. Streamlit에서는 화면을 넣을 장소를 지정할 때 자주 사용합니다.

---

**8. `sidebar`: 화면 옆에 보조 공간 만들기**

```python
with st.sidebar:
    st.title("우리 가게")
    st.write("메뉴를 골라주세요.")
```

내용이 화면 옆의 사이드바에 들어갑니다.

우리 주문 관리 화면에서는 여기에 다음을 넣었어요.

- 관리자 이름
- 메뉴 선택
- 조회 기간
- 취소·환불 숨기기

자주 사용하는 선택 도구를 옆에 모아둔 것입니다.

---

**9. `container`: 여러 내용을 한 상자에 모으기**

```python
with st.container():
    st.write("첫 번째 내용")
    st.write("두 번째 내용")
```

여러 화면 요소를 묶는 상자예요.

채팅 화면에서는 높이를 정했습니다.

```python
with st.container(height=550, border=False):
    st.write("여기에 대화가 들어갑니다.")
```

- `height=550`: 상자의 높이를 550픽셀로 지정
- `border=False`: 테두리를 표시하지 않음

대화가 많아지면 정해진 영역 안에서 스크롤할 수 있어요.

---

**10. 입력 도구는 사용자의 답을 돌려줘요**

지금까지는 우리가 사용자에게 보여주었습니다.

이제는 사용자가 우리에게 답하는 도구를 배울 차례예요.

```python
name = st.text_input("이름")
```

이 코드는 두 가지 일을 합니다.

1. 이름을 입력하는 칸을 만들어요.
2. 입력한 글을 `name`에 담아요.

`name`처럼 값을 담아두는 이름을 **변수**라고 합니다.

```text
입력 칸에 민수 입력
        ↓
name에 "민수"가 담김
```

`=`는 여기서 “같다”라는 질문이 아니라 **오른쪽 값을 왼쪽 이름에 담는다**는 뜻이에요.

---

**11. `text_input`과 `text_area`: 글을 입력받기**

`text_input`은 한 줄짜리 입력 칸입니다.

```python
name = st.text_input("이름")
st.write(name)
```

`text_area`는 여러 줄을 쓸 수 있는 입력 칸입니다.

```python
story = st.text_area("오늘 있었던 일을 적어주세요.")
st.write(story)
```

| 함수 | 주로 쓰는 곳 |
|---|---|
| `text_input` | 이름, 제목, 검색어 |
| `text_area` | 긴 설명, 게시글 내용 |

입력 안내도 넣을 수 있습니다.

```python
name = st.text_input(
    "이름",
    placeholder="이름을 입력하세요",
)
```

`placeholder`는 **아직 입력하지 않았을 때 보여주는 안내문**입니다. 실제 입력값은 아니에요.

반면:

```python
name = st.text_input("이름", value="민수")
```

`value`는 처음부터 입력 칸에 들어 있는 실제 값입니다.

---

**12. `radio`: 펼쳐진 선택지에서 하나 고르기**

```python
fruit = st.radio(
    "좋아하는 과일",
    ["사과", "바나나", "딸기"],
)

st.write(fruit)
```

여러 선택지가 펼쳐져 있고, 그중 하나를 고릅니다.

딸기를 고르면 `fruit`에는 `"딸기"`가 담겨요.

우리 메뉴도 같은 원리입니다.

```python
menu = st.radio(
    "메뉴",
    ["대시보드", "주문 관리", "상품 관리", "정산"],
    index=1,
)
```

`index=1`은 처음에 두 번째 항목을 선택한다는 뜻입니다.

파이썬의 위치 번호는 보통 **0부터** 시작해요.

```text
0 → 대시보드
1 → 주문 관리
2 → 상품 관리
3 → 정산
```

---

**13. `selectbox`: 접힌 목록에서 하나 고르기**

```python
fruit = st.selectbox(
    "과일",
    ["사과", "바나나", "딸기"],
)
```

누르면 목록이 펼쳐지고, 하나를 고를 수 있어요.

`radio`와 `selectbox`는 둘 다 하나를 고릅니다.

| 도구 | 선택지의 모습 |
|---|---|
| `radio` | 처음부터 펼쳐져 있음 |
| `selectbox` | 눌렀을 때 펼쳐짐 |

우리는 조회 기간, 주문 상태, 상품 카테고리에 사용했습니다.

---

**14. `multiselect`: 여러 개 고르기**

```python
fruits = st.multiselect(
    "먹고 싶은 과일",
    ["사과", "바나나", "딸기"],
    default=["사과"],
)

st.write(fruits)
```

하나도 고르지 않아도 되고, 여러 개를 골라도 됩니다.

사과와 딸기를 고르면:

```python
["사과", "딸기"]
```

아무것도 고르지 않으면:

```python
[]
```

처럼 값이 나옵니다.

이렇게 여러 값을 순서대로 담은 것을 **리스트**라고 해요.

`default`는 처음부터 선택해둘 값입니다.

---

**15. `checkbox`: 켜거나 끄기**

```python
show_price = st.checkbox("가격 보기")
```

체크박스가 돌려주는 값은 두 가지뿐입니다.

| 상태 | 값 | 뜻 |
|---|---|---|
| 체크함 | `True` | 참, 켜짐 |
| 체크하지 않음 | `False` | 거짓, 꺼짐 |

조건문과 함께 쓰면:

```python
if show_price:
    st.write("가격은 1,000원입니다.")
```

체크했을 때만 가격이 나타납니다.

우리 앱의 “취소·환불 숨기기”도 같은 원리예요.

---

**16. `if`: 조건이 맞을 때만 실행하기**

`if`는 “만약에”라는 뜻입니다.

```python
if menu == "정산":
    st.write("정산 화면입니다.")
```

“메뉴가 정산이라면 이 글을 보여줘”라는 뜻이에요.

자주 사용하는 조건을 비교해 볼게요.

| 코드 | 뜻 |
|---|---|
| `menu == "정산"` | 메뉴가 정산인가? |
| `menu != "정산"` | 메뉴가 정산이 아닌가? |
| `menu in ["대시보드", "주문 관리"]` | 메뉴가 두 항목 중 하나인가? |
| `menu not in ["정산", "상품 관리"]` | 메뉴가 두 항목 모두 아닌가? |

`=`와 `==`는 다릅니다.

```python
menu = "정산"       # 값을 담기
menu == "정산"      # 같은지 확인하기
```

우리가 사용한 조건을 표로 정리하면:

| 선택 메뉴 | `menu != "정산"` | `menu in ["대시보드", "주문 관리"]` |
|---|---|---|
| 대시보드 | 실행함 | 실행함 |
| 주문 관리 | 실행함 | 실행함 |
| 상품 관리 | 실행함 | 건너뜀 |
| 정산 | 건너뜀 | 건너뜀 |

이것이 앞서 이해하신 **메뉴에 따라 화면 구성을 바꾸는 원리**입니다.

---

**17. `button`: 누르는 순간 일을 시키기**

```python
clicked = st.button("인사하기")

if clicked:
    st.write("안녕하세요!")
```

버튼을 누르면, 그 클릭으로 실행된 회차에서 `clicked`가 `True`가 됩니다.

여기서 중요한 차이가 있어요.

- 체크박스는 체크 상태를 유지합니다.
- 버튼은 눌린 순간의 사건을 알려줍니다.

따라서 다음 코드는 인사말을 영구 저장하지 않아요.

```python
if st.button("인사하기"):
    st.write("안녕하세요!")
```

다른 위젯을 조작해 다시 실행하면 버튼 값은 보통 `False`이므로 인사말이 사라질 수 있습니다.

계속 기억하게 하려면 뒤에서 배울 `session_state`가 필요해요.

---

**18. `form`: 입력을 모아 보내는 주문서**

상품 등록에는 여러 입력이 필요합니다.

```text
상품코드
상품명
가격
재고
```

이것을 한 장의 주문서처럼 묶는 것이 `form`입니다.

```python
with st.form("product_form"):
    name = st.text_input("상품명")
    price = st.text_input("가격")
    submitted = st.form_submit_button("등록")

if submitted:
    st.write(name)
    st.write(price)
```

폼 안의 입력은 **제출 버튼을 눌렀을 때 함께 전달**됩니다. 따라서 한 항목씩 수정할 때마다 앱 전체에 반영하는 대신, 작성한 내용을 모아서 처리할 수 있어요. [공식 폼 설명](https://docs.streamlit.io/develop/concepts/architecture/forms)

`"product_form"`은 이 폼을 구별하기 위한 이름입니다.

`form_submit_button`은 **폼 전용 제출 버튼**이에요. 폼 안에서는 일반 `button` 대신 이것을 사용합니다.

또한 제출 버튼이 있다고 자동으로 저장되는 것은 아닙니다.

```python
if submitted:
    # 여기에 검사하거나 저장하는 코드를 작성해야 합니다.
    st.success("제출을 받았습니다.")
```

---

**19. `file_uploader`: 파일 받기**

```python
uploaded = st.file_uploader(
    "상품 사진",
    type=["png", "jpg"],
)
```

사용자가 파일을 고를 수 있는 도구입니다.

- 선택 전: `None`
- 선택 후: 업로드된 파일 객체

`None`은 “아직 값이 없다”라고 생각하면 됩니다.

```python
if uploaded is not None:
    st.write(uploaded.name)
```

파일을 골랐을 때 이름을 보여주는 코드예요.

파일을 받는 것과 영구 저장하는 것은 다릅니다. 우리 앱은 받은 이미지를 현재 세션에 보관하도록 별도의 코드를 넣었어요.

---

**20. `tabs`: 이름표를 눌러 구역 바꾸기**

```python
tab1, tab2 = st.tabs(["상품 등록", "도움말"])

with tab1:
    st.write("상품 등록 내용")

with tab2:
    st.write("도움말 내용")
```

공책에 붙여놓은 두 개의 색인표처럼 생각해 보세요.

이름표를 누르면 해당 구역이 보입니다.

다만 우리가 사용한 기본 방식에서는 **선택하지 않은 탭 안의 코드도 실행됩니다.**

앞에서 배운 `if`와 다릅니다.

```python
if menu == "상품 관리":
    # 조건이 거짓이면 이 코드는 실행하지 않습니다.
    ...
```

탭은 기본적으로 내용을 나눠 보여주는 도구이고, `if`는 코드 실행 여부를 결정하는 문법입니다.

---

**21. `expander`: 열고 닫는 설명 상자**

```python
with st.expander("자세히 보기"):
    st.write("여기에 자세한 설명이 들어갑니다.")
```

처음에는 제목만 보이고, 누르면 안의 내용이 펼쳐집니다.

우리는 최근 처리 로그를 여기에 넣었어요.

탭처럼, 우리가 사용한 기본 방식에서는 **상자를 닫아도 안의 코드는 실행됩니다.** 화면에서 접혀 있을 뿐이에요.

---

**22. `metric`: 중요한 숫자를 크게 보여주기**

```python
st.metric(
    label="오늘 주문",
    value=128,
    delta="+12",
)
```

| 재료 | 의미 |
|---|---|
| `label` | 무엇을 나타내는 숫자인지 |
| `value` | 크게 보여줄 값 |
| `delta` | 비교할 때 얼마나 달라졌는지 |

예를 들어:

```text
오늘 주문
128
↑ +12
```

처럼 표시합니다.

주의할 점은 **`metric`이 주문 수를 직접 계산해주지는 않는다**는 거예요.

우리가 `128`을 주면 128을 보여주고, 계산한 결과를 주면 그 결과를 보여줍니다.

---

**23. `dataframe`: 데이터를 표로 보여주기**

먼저 표에 넣을 데이터가 필요합니다.

```python
import pandas as pd

table = pd.DataFrame({
    "상품": ["사과", "바나나"],
    "가격": [1000, 2000],
})
```

`pandas`는 데이터를 다루는 별도의 도구예요.

`pd.DataFrame`은 행과 열이 있는 표 형태의 데이터를 만듭니다.

```text
상품      가격
사과      1000
바나나    2000
```

이제 화면에 표시합니다.

```python
st.dataframe(table, hide_index=True)
```

역할을 나누면:

- `pd.DataFrame`: 표 데이터를 만들기
- `st.dataframe`: 표 데이터를 화면에 보여주기

`hide_index=True`는 왼쪽의 행 번호를 숨기는 설정입니다.

`dataframe`은 기본적으로 보여주는 표예요. 값을 직접 수정하게 하는 `data_editor`는 별도의 도구입니다.

---

**24. `column_config.NumberColumn`: 숫자 열의 표시 방식 정하기**

가격이 그냥 `1000`으로 나오면 조금 딱딱하죠.

```python
st.dataframe(
    table,
    hide_index=True,
    column_config={
        "가격": st.column_config.NumberColumn(
            "가격",
            format="%,d원",
        )
    },
)
```

이렇게 하면 가격을 `1,000원`처럼 표시할 수 있어요.

나눠 읽으면:

```text
column_config
→ 표의 열을 어떻게 보여줄지 정하기

"가격"
→ 가격 열에 적용하기

NumberColumn
→ 숫자 열의 설정 만들기

format="%,d원"
→ 정수에 천 단위 쉼표와 ‘원’을 붙여 표시하기
```

이 설정은 숫자의 **보이는 모습**을 바꿉니다. 원본 숫자 `1000`을 글자 `"1,000원"`으로 바꾸는 것은 아니에요. [공식 숫자 열 설명](https://docs.streamlit.io/develop/api-reference/data/st.column_config/st.column_config.numbercolumn)

---

**25. `bar_chart`: 숫자를 막대 길이로 보여주기**

```python
table = pd.DataFrame({
    "상품": ["사과", "바나나", "딸기"],
    "주문 수": [3, 5, 2],
})

st.bar_chart(
    table,
    x="상품",
    y="주문 수",
)
```

주문이 많을수록 막대가 길어져요.

가로 막대를 원하면:

```python
st.bar_chart(
    table,
    x="상품",
    y="주문 수",
    horizontal=True,
)
```

우리 앱의 카테고리별 주문 수가 이런 방식입니다.

차트 역시 숫자를 직접 알아내지는 않습니다. **계산한 데이터를 주면 그림으로 표현**해 줍니다.

---

**26. `progress`: 얼마나 끝났는지 보여주기**

```python
st.progress(0.7, text="열 개 중 일곱 개 완료")
```

막대를 70%만큼 채웁니다.

소수로 표현할 때:

| 값 | 진행 정도 |
|---|---|
| `0.0` | 0% |
| `0.5` | 50% |
| `1.0` | 100% |

정수 `0`부터 `100`을 사용할 수도 있어요.

```python
st.progress(70)
```

우리 앱에서는:

```python
st.progress(69 / 96)
```

96건 중 69건이 완료됐다는 비율을 넣었습니다.

이 막대는 작업을 직접 수행하거나 자동으로 추적하지 않아요. 코드가 알려준 진행 정도를 보여줍니다.

---

**27. `info`, `warning`, `success`: 안내 표지판**

```python
st.info("상품을 골라주세요.")
st.warning("이름을 입력하지 않았어요.")
st.success("등록이 완료됐어요.")
```

| 함수 | 용도 |
|---|---|
| `info` | 알아두면 좋은 안내 |
| `warning` | 확인하거나 고쳐야 할 내용 |
| `success` | 작업이 잘 끝났다는 안내 |

중요한 점이 있습니다.

```python
st.warning("이름이 없어요.")
```

이것만으로는 등록을 막을 수 없어요. 안내문만 보여줍니다.

막는 로직은 조건문으로 작성해야 합니다.

```python
if name == "":
    st.warning("이름을 입력해주세요.")
else:
    st.success("입력을 확인했어요.")
```

여기서 `else`는 “그렇지 않으면”이라는 뜻입니다.

---

**28. `chat_message`: 대화 표시 상자**

```python
with st.chat_message("user"):
    st.write("안녕하세요!")

with st.chat_message("assistant"):
    st.write("반가워요!")
```

- `"user"`: 사용자 메시지
- `"assistant"`: 도우미 메시지

메시지를 대화 형식으로 보여주는 역할입니다.

**이 함수 자체가 AI를 불러오는 것은 아닙니다.**

우리가 글을 주면, 그 글을 대화처럼 표시해 줍니다.

---

**29. `chat_input`: 대화 입력창**

```python
message = st.chat_input("메시지를 입력하세요")

if message:
    with st.chat_message("user"):
        st.write(message)
```

사용자가 메시지를 제출하면 그 글을 받을 수 있어요.

흐름은 다음과 같습니다.

```text
메시지 작성
    ↓
전송
    ↓
message에 글이 들어옴
    ↓
chat_message로 표시
```

이 코드만으로는 이전 대화를 모두 기억하지는 않습니다.

이제 기억하는 도구를 배워봅시다.

---

**30. `session_state`: 다시 실행해도 기억하는 보관함**

다음 코드를 보세요.

```python
count = 0

if st.button("하나 추가"):
    count = count + 1

st.write(count)
```

계속 누르면 1, 2, 3이 될 것 같지만 그렇지 않아요.

버튼을 누를 때마다:

1. 코드가 다시 실행됩니다.
2. `count = 0`이 다시 실행됩니다.
3. 1을 더합니다.
4. 다시 1이 됩니다.

그래서 재실행 사이에도 값을 기억할 보관함이 필요합니다.

```python
if "count" not in st.session_state:
    st.session_state.count = 0

if st.button("하나 추가"):
    st.session_state.count += 1

st.write(st.session_state.count)
```

첫 부분을 읽어볼까요?

```python
if "count" not in st.session_state:
```

“보관함에 아직 `count`가 없다면”

```python
st.session_state.count = 0
```

“처음 한 번 0을 넣어줘.”

이미 있으면 초기화하지 않기 때문에 숫자가 계속 증가합니다.

`+= 1`은 기존 값에 1을 더한다는 뜻이에요.

우리 앱에서는 이 보관함에:

- 대화 목록
- 게시글 목록
- 등록 상품 목록

을 넣었습니다.

다만 `session_state`는 **현재 연결된 세션의 기억**입니다. 영구 저장소가 아니며, 새 세션에서는 초기화될 수 있어요. [공식 세션 상태 설명](https://docs.streamlit.io/develop/concepts/architecture/session-state)

---

**31. `key`: 입력 도구의 이름표**

```python
st.text_input("이름", key="customer_name")
```

`"이름"`은 사용자에게 보이는 설명입니다.

`"customer_name"`은 코드에서 도구를 구별하는 이름표예요.

이렇게 접근할 수 있습니다.

```python
st.write(st.session_state.customer_name)
```

같은 종류의 입력 칸을 여러 개 만들 때도 도움이 됩니다.

```python
st.text_input("이름", key="buyer_name")
st.text_input("이름", key="receiver_name")
```

겉으로는 둘 다 “이름”이지만, 내부 이름표는 다릅니다.

다만 위젯에 연결된 값은 그 위젯이 화면에서 빠질 때 정리될 수 있어요. 오래 유지해야 하는 실제 데이터는 우리 앱의 `admin_products`처럼 별도의 세션 값에 보관하는 것이 좋습니다. [공식 위젯 동작 설명](https://docs.streamlit.io/develop/concepts/architecture/widget-behavior)

---

**32. `on_click`, `on_submit`: 일이 생기면 부를 함수**

먼저 우리가 직접 도구를 만들 수도 있습니다.

```python
def say_hello():
    st.session_state.greeting = "안녕하세요!"
```

`def`는 “이런 일을 하는 함수를 만들게”라는 파이썬 문법이에요.

이제 버튼과 연결합니다.

```python
st.button("인사하기", on_click=say_hello)
```

뜻은:

> 버튼을 누르면 `say_hello`를 실행해줘.

이처럼 특정 일이 생겼을 때 호출하는 함수를 **콜백**이라고 합니다.

- `on_click`: 버튼 클릭 시 호출
- `on_submit`: 채팅 제출 시 호출
- `on_change`: 지원하는 입력 위젯의 값 변경 시 호출

우리 예제의 일반적인 순서는 다음과 같아요.

```text
사용자가 입력하거나 버튼을 누름
        ↓
새 입력값 반영
        ↓
연결된 콜백 실행
        ↓
본문 코드를 위에서부터 재실행
```

콜백에서 먼저 데이터를 저장하니, 본문이 다시 실행될 때 최신 내용을 표시할 수 있습니다.

여기에는 괄호를 붙이지 않는다는 점도 기억하세요.

```python
on_click=say_hello
```

함수 자체를 전달하여 “나중에 불러줘”라고 하는 것입니다.

```python
on_click=say_hello()
```

처럼 쓰면 함수를 지금 실행하고 그 결과를 전달하므로 의미가 달라집니다.

---

**33. 화면 도구와 실제 일을 하는 코드를 구별하기**

이 구별을 이해하면 코드가 훨씬 선명하게 보입니다.

| 화면 도구 | 도구 밖에서 작성해야 하는 일 |
|---|---|
| `text_input` | 입력한 글이 올바른지 검사하기 |
| `button` | 눌렀을 때 무엇을 할지 결정하기 |
| `form_submit_button` | 제출한 상품을 목록에 저장하기 |
| `dataframe` | 표시할 주문을 검색하고 골라내기 |
| `metric` | 주문 수나 합계를 계산하기 |
| `bar_chart` | 카테고리별 주문 수를 계산하기 |
| `progress` | 실제 완료 비율을 계산하기 |
| `success` | 실제 저장 작업을 수행하기 |
| `chat_message` | 답변 내용을 만들기 |

예를 들어 검색창은 검색어를 받아줄 뿐이에요.

```python
search = st.text_input("검색")
```

그다음 **어떤 주문이 검색어와 맞는지 고르는 코드**를 우리가 작성해야 검색 기능이 됩니다.

또한:

```python
st.success("저장 완료!")
```

라고 썼다고 데이터가 저장되지는 않아요. 실제 저장이 끝난 다음 이 문구를 보여줘야 합니다.

---

**34. 지금까지 배운 것을 작은 예제로 연결하기**

아래 코드는 설명용입니다. 파일을 새로 만들거나 수정한 것은 아니에요.

```python
import streamlit as st

st.set_page_config(page_title="과일 가게")

# 처음 한 번 장바구니를 준비합니다.
if "basket" not in st.session_state:
    st.session_state.basket = []

# 옆에서 메뉴를 고릅니다.
with st.sidebar:
    menu = st.radio(
        "메뉴",
        ["과일 담기", "장바구니"],
    )

st.title("🍎 과일 가게")

# 선택에 따라 다른 코드를 실행합니다.
if menu == "과일 담기":
    with st.form("fruit_form"):
        fruit = st.selectbox(
            "과일을 골라주세요",
            ["사과", "바나나", "딸기"],
        )

        submitted = st.form_submit_button("담기")

    if submitted:
        st.session_state.basket.append(fruit)
        st.success("과일을 담았어요!")

elif menu == "장바구니":
    st.subheader("내가 담은 과일")

    if st.session_state.basket:
        st.write(st.session_state.basket)
    else:
        st.info("아직 담은 과일이 없어요.")
```

새로운 표현 두 가지가 있습니다.

- `append(fruit)`: 목록 끝에 과일 하나를 추가
- `elif`: 앞의 조건이 맞지 않으면 다른 조건을 확인

이제 동작을 따라가 보세요.

1. 처음에는 빈 장바구니 `[]`를 준비합니다.
2. 사용자가 사과를 고릅니다.
3. “담기”를 누릅니다.
4. 폼의 입력값이 전달되고 코드가 재실행됩니다.
5. 기존 장바구니는 `session_state`에 있으므로 유지됩니다.
6. 사과를 장바구니에 추가합니다.
7. 메뉴를 “장바구니”로 바꾸면 다시 실행됩니다.
8. 이번에는 장바구니를 보여주는 조건에 들어갑니다.

이 예제에서 **입력 도구는 선택을 받고, 조건문은 실행할 부분을 고르고, 세션 상태는 결과를 기억하고, 출력 도구는 결과를 보여줍니다.**

마지막으로 직접 생각해 볼 질문입니다.

```python
if menu != "장바구니":
    st.write("과일을 골라주세요.")
```

메뉴가 `"장바구니"`일 때 이 문장이 안 나오는 이유는 무엇일까요?

**조건이 거짓이어서 `st.write`가 실행되지 않기 때문**입니다. 지금 이해하신 메뉴 전환 원리가 다른 기능들과도 이렇게 연결됩니다.