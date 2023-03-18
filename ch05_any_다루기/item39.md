# any를 구체적으로 변형해서 사용하기

- 함수의 매개변수가 배열이지만 값을 알 수 없을 때

```ts
// Bad
function getLengthBad(array: any) {
  return array.length; // 반환값이 any
}

// Good
function getLength1(array: any[]) {
  return array.length; // 반환값이 number
}

// Good
function getLength2(array: any[][]) { // 배열의 배열 형태일 때
  return array.length;
}
```

- 함수의 매개변수가 객체이지만 값을 알 수 없을 때

```ts
function hasTwelveLetterKey(o: {[key: string]: any}) {
  for (const key in o) {
    if(key.length === 12) {
      return true;
    }
  }
  return false;
}
```

- 함수의 타입을 구체화

```ts
type Fn0 = () => any; // 매개변수 없이 호출 가능한 모든 함수
type Fn1 = (arg: any) => any; // 매개변수 1개
type FnN = (...args: any[]) => any; // 모든 개수의 매배견수 "Function" 타입과 동일
```