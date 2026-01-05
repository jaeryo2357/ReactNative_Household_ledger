# ReactNative_Household_ledger

## JavaScript 반올림

```js
function financial(x) {
  return Number.parseFloat(x).toFixed(2);
}

console.log(financial(123.456));
// Expected output: "123.46"
```

## Navigation 화면 Presentation 종류

Navigator 종류에 따라서 지원하는 presentation이 다름.
animation 보다는 화면의 성격? 스타일을 정의?

card: 옆에서 새로운 화면이 전환되는 방식
modal: 아래에서 새로운 화면이 전환되는 방식
formSheet: BottomSheetBehavior 느낌으로 화면 전환

```js
    <Stack.Screen
        name="ManageExpense" component={ManageExpense}
            options={{
              presentation: 'modal',
        }}
    /> 
```

## TextInput properties 살펴보기

autoCorrect, autoCapitalize, placeHolder 등의 properties를 다룸.

```js

  <TextInput
      multiline={true}
```
iOS는 top, Android center 정렬되므로, `textAlignVertical` top을 설정하여 각 플랫폼이 동일하게 나오도록 구현하자.

공식 문서의 Note 유심히 보기

## TextInput 여러개의 입력을 묶어서 관리하기

각 TextInput 마다 state를 별도로 관리해도 되고, 아래처럼 하나의 객체로 관리해도 좋다.

```js
  const [inputValues, setInputValues] = useState({
    amount: '',
    date: '',
    description: '',
  });

  function inputChangedHandler(inputIdentifier, enteredValue) {
    setInputValues((curInputValues) => {
      return {
        ...curInputValues,
        [inputIdentifier]: enteredValue,
      };
    });
  }

  ...

  <Input
          style={styles.rowInput}
          label="Amount"
          textInputConfig={{
            ...
            onChangeText: inputChangedHandler.bind(this, 'amount', value),
            value: inputValues.amount,
          }}
        />

```

## 문자열 숫자로 변환하기

```js
+amount
```
+ 기호를 앞에 붙이면 문자열이 숫자로 변환된다.

---

# Network 통신

## fetch 
Javascript 브라우저에서 쓰는 `fetch`, RN에서도 사용 가능
https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch

예제
```js
async function logJSONData() {
  const response = await fetch("http://example.com/movies.json");
  const jsonData = await response.json();
  console.log(jsonData);
}
```

post, put, delete 요청도 가능
```js
  const response = await fetch(url, {
    method: "POST", // *GET, POST, PUT, DELETE
    ...
    headers: {
      "Content-Type": "application/json",
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    ...
    body: JSON.stringify(data), // body의 데이터 유형은 반드시 "Content-Type" 헤더와 일치해야 함
  });
  return response.json(); // JSON 응답을 네이티브 JavaScript 객체로 파싱
}
```

## Axios

강의에서는 XML 요청도 지원 가능한 Axios를 적용.
찾아보니 XML 요청 뿐만 아니라 인터셉터, 에러 처리, 타임 아웃 등 여러 헬퍼 기능 많음

https://www.inflearn.com/blogs/8023?gad_source=1&gad_campaignid=20714471420&gbraid=0AAAAADAClSCiVb36ugTo6B4PTehA_o_da&gclid=CjwKCAiA3-3KBhBiEiwA2x7FdDv7LgBaEfKkdyanlTR3YuF6XbB6mGzivgCVLOwoxoE5mQ1wWodBkxoC7jIQAvD_BwE

예제 코드

```js
export async function fetchExpenses() {onst response = await axios.get(BACKEND_URL + '/expenses.json');

  const expenses = [];

  for (const key in response.data) {
    const expenseObj = {
      id: key,
      amount: response.data[key].amount,
      date: new Date(response.data[key].date),
      description: response.data[key].description
    };
    expenses.push(expenseObj);
  }

  return expenses;
}
```

## async await 문법

https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await

강의에서 가볍게 넘어간 async, await

JavaScript에서 비동기를 처리하는 방식은 크게 3가지 콜백, promiss, async-await

1. 콜백
```js
getData (function (x) {
  getMoreData (x, function (y) {
    getMoreData (y, function (z) {
      ...
    });
  });
});
```

2. Promiss
```js
fetch('https://example.com/api')
  .then(response => response.json())
  .then(data => fetch(`https://example.com/api/${data.id}`))
  .then(response => response.json())
  .then(data => fetch(`https://example.com/api/${data.id}/details`))
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

체이닝 방식으로 개선이 되었지만 여전히 헬

3. async-await
```js
async function getData() {
    const response = await fetch('https://example.com/api');
    const data = await response.json();
    const response2 = await fetch(`https://example.com/api/${data.id}`);
    const data2 = await response2.json();
    const response3 = await fetch(`https://example.com/api/${data.id}/details`);
    const data3 = await response3.json();
    console.log(data3);
}
```

비동기를 동기 코드로 작성 가능함.

사용 방법은 비동기 promiss 작업을 수행할 함수 선언에 `async`를 선언
비동기로 실행하는 코드를 기다리고 싶을때 `await` 사용

단, async-await은 promiss를 대체하는 것은 아니다. 내부적으로 promiss를 여전히 사용 중.
다만 표현을 좀 더 가독성 좋도록 보여주는 것임
