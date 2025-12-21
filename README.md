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