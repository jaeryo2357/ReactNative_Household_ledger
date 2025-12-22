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
            onChangeText: inputChangedHandler.bind(this, 'amount'),
            value: inputValues.amount,
          }}
        />

```

## 문자열 숫자로 변환하기

```js
+amount
```

+ 기호를 앞에 붙이면 문자열이 숫자로 변환된다.
