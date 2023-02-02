# number 인덱스 시그니처보다는 Array, 튜플, ArrayLike를 사용하기

javascript의 객체에는 이상한 점이 몇가지 있습니다.
```js
const x = {};
x[[1,2,3]] = 2;
console.log(x) // {'1,2,3': 2}
```
```js
const x = {1: 2, 3: 4};
console.log(x) // {'1': 2, '3': 4}
```
javascript에서 객체의 key로는 문자열만 사용 가능합니다. (es6 이후로는 symbol도 가능)

```js
const x = [1, 2, 3]
console.log(x['0']) // 1
console.log(Object.keys(x)) // ['0', '1', '2']
```
javascript에서 배열은 객체이며 숫자 인덱스가 사용가능한것 처럼 보여도, 내부적으로는 문자열로 변환되어 사용됩니다.

타입스크립트는 이러한 혼란을 바로잡기 위해 숫자 키를 허용하고, 문자열 키와 다른 것으로 구분합니다.
```ts
interface Array<T> {
  [n: number]: T;
}
```
```ts
const xs = [1, 2, 3];
const x0 = xs[0]; // OK
const x1 = xs['1']; // Error
```
런타임에는 소용이 없는 가상의 정의이지만, 타입 체크 시점에 오류를 잡을 수 있어 유용합니다.

```ts
const xs: Array<number> = [1, 2, 3]; // 타입이 string[]
const keys = Object.keys(xs);
for (const key in xs) {
  console.log(typeof key);  // 타입이 string
  console.log(xs[key]);  // 타입이 1, 2, 3
}
```
런타임에서는 여전히 key가 문자열로 변환됩니다.

이는 혼란을 불어일으킬 수 있습니다. 또한 push, concat등 내부 프로퍼티나 메소드를 사용할 때 오류를 발생시킬 수 있습니다.

인덱스 시그니처로 number를 사용할 일은 많지 않기 때문에 배열 타입을 선언 할 때는 인덱스 시그니처 대신 Array 또는 튜플 타입을 사용하는것이 좋습니다.

```ts
interface ArrayLike<T> {
  readonly length: number;
  readonly [n: number]: T;
}
```
arguments, HTMLCollection 과 같은 유사 배열 객체(Array-like object)를 사용하고 싶다면 lib.es5.d.ts 정의한 ArrayLike 타입을 사용하면 됩니다. 그러나 여전히 런타임에서는 키가 string이라는 점은 주의해야 합니다.

## 요약

- 배열은 객체이므로 키는 숫자가 아니라 문자열입니다. 인덱스 시그니처로 사용된 number타입은 버그를 잡기 위한 순수 타입스크립트 코드입니다.
- 인덱스 시그니처에 number를 사용하기보다 Array나 튜플, 또는 ArrayLike 타입을 사용하는 것이 좋습니다.