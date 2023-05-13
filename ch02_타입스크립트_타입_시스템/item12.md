# 함수 표현식에 타입 적용하기

타입 스크립트에서는 함수 표현식을 사용하는 것이 좋다.

```ts
function rollDice1(sides: number): number { /* ... */ } // 문장
const rollDice2 = function (sides: number): number { /* ... */ }; // 표현식
const rollDice3 = (sides: number): number => { /* ... */ }; // 표현식
```

함수의 매개변수부터 반환값까지 전체를 함수 타입으로 선언하여 함수 표현식에 재사용할 수 있다는 장점이 있다.

```ts
type DiceRollFn = (sides: number) => number;
const rollDice: DiceRollFn = (sides) => {
  return 0;
};
```

반복되는 함수를 하나의 타입으로 통합할 수 있다.

```ts
type BinaryFn = (a: number, b: number) => number;
const add: BinaryFn = (a, b) => a + b;
const sub: BinaryFn = (a, b) => a - b;
const mul: BinaryFn = (a, b) => a * b;
const div: BinaryFn = (a, b) => a / b;
```

함수의 매개변수에 타입을 선언하는 것보다 함수 표현식 전체 타입을 정의하는 것이 코드도 간결하고 안전하다.

```ts
// Bad
async function checkedFetch(input: RequestInfo, init?: RequestInit) {
  const response = await fetch(input, init);
  if (!response.ok) {
    throw new Error("Request failed: " + response.status); //return으로 바뀌어도 오류를 잡아내지 못함
  }
  return response;
}

// Good
const checkedFetch: typeof fetch = async (input, init) => {
  const response = await fetch(input, init);
  if (!response.ok) {
    throw new Error("Request failed: " + response.status); // return으로 바뀌면 오류를 잡아냄
  }
  return response;
};
```

다른 함수의 시그니처를 사용하기 위해 typeof fn을 사용.
