# 변경 관련된 오류 방지를 위해 readonly 사용하기

- `readonly number[]`타입이 `number[]` 타입과 구분되는 특징

  - 배열의 요소를 읽을 수 있지만, 쓸 수는 없다
  - length를 읽을 수 있지만, 바꿀 수 없다(배열을 변경함)
  - 배열을 변경하는 pop을 비롯한 다른 메서드를 호출할 수 없다

- `number[]`는 `readonly number[]`보다 기능이 많기 때문에, `readonly number[]`의 서브타입이 된다

```ts
const a: number[] = [1, 2, 3];
const b: readonly number[] = a;
const c: number[] = b;
// ~ Type 'readonly number[]' is 'readonly' and cannot be
//   assigned to the mutable type 'number[]'
```

- 매개변수를 `readonly`로 선언했을 때 생기는 일

  - 타입스크립트는 매개변수가 함수 내에서 변경이 일어나는지 체크합니다
  - 호출하는 쪽에서는 함수가 매개변수를 변경하지 않는다는 보장을 받게 됩니다
  - 호출하는 쪽에서 함수에 readonly 배열을 매개변수로 넣을 수도 있습니다

- `readonly`는 얕게(shallow) 동작한다는 것에 유의하며 사용해야 합니다. 만약 객체의 `readonly` 배열이 있다면, 그 객체 자체는 `readonly`가 아닙니다.

```ts
const dates: readonly Date[] = [new Date()];
dates.push(new Date());
// ~~~~ Property 'push' does not exist on type 'readonly Date[]'
dates[0].setFullYear(2037); // OK
```

- 비슷한 경우가 `readonly`의 사촌 격이자 객체에 사용되는 `Readonly` 제너릭에도 해당됩니다.

```ts
interface Outer {
  inner: {
    x: number;
  };
}
const o: Readonly<Outer> = { inner: { x: 0 } };
o.inner = { x: 1 };
// ~~~~ Cannot assign to 'inner' because it is a read-only property
o.inner.x = 1; // OK
```

중요한 점은 `readonly` 접근제어자에는 `inner`에 적용되는 것이지 `x`는 아니라는 것입니다.

```ts
interface Outer {
  inner: {
    x: number;
  };
}
const o: Readonly<Outer> = { inner: { x: 0 } };
o.inner = { x: 1 };
// ~~~~ Cannot assign to 'inner' because it is a read-only property
o.inner.x = 1; // OK
type T = Readonly<Outer>;
// Type T = {
//   readonly inner: {
//     x: number;
//   };
// }
```

- 제너릭을 만들면 깊은 `readonly` 타입을 사용할 수 있습니다. 그러나 제너릭은 만들기 까다롭기 때문에 라이브러리를 사용하는 게 낫습니다. 예를 들어 ts-essentials에 있는 DeepReadonly 제너릭을 사용하면 됩니다.

## 요약

- 만약 함수가 매개변수를 수정하지 않는다면 readonly로 선언하는 것이 좋습니다. readonly 매개변수는 인터페이스를 명확하게 하며, 매개변수가 변경되는 것을 방지합니다.
- readonly를 사용하면 변경하면서 발생하는 오류를 방지할 수 있고, 변경이 발생하는 코드도 쉽게 찾을 수 있습니다.
- const와 readonly의 차이를 이해해야 합니다.
- readonly는 얕게 동작한다는 것을 명심해야 합니다.
