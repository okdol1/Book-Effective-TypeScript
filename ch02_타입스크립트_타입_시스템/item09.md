# 타입 단언보다는 타입 선언을 사용하기

### 타입 선언 vs 타입 단언

```ts
interface Person {
  name: string;
}

const alice: Person = { name: "Alice" }; // -> 타입 선언
const bob = { name: "Bob" } as Person; // -> 타입 단언
```

```ts
const alice: Person = {};
// ~~~~~ 'Person' 유형에 필요한 'name' 속성이 '{}' 유형에 있습니다.
const bob = {} as Person; // 오류 없음
```

- 타입 선언은 할당되는 값이 해당 인터페이스를 만족하는지 검사한다.
- 타입 단언은 강제로 타입을 지정했으니 타입 체커에게 오류를 무시하라고 하는 것이다.

타입 속성을 추가할 때도 마찬가지다.

```tsx
const alice: Person = {
  name: "Alice",
  occupation: "TypeScript developer",
  // ~~~~~~~~~ 개체 리터럴은 알려진 속성만 지정할 수 있으며, 'Person' 형식에 'occupation'이 없습니다.
};
const bob = {
  name: "Bob",
  occupation: "JavaScript developer",
} as Person; // 오류 없음
```

화살표 함수의 타입선언은 추론된 타입이 모호할 경우가 있다.

```ts
const people = ["alice", "bob", "jan"].map((name) => ({ name }));
// Person[]을 원했지만 경과는 {name: string;}[]...
```

단언문을 사용하면 문제가 해결되는 것처럼 보이지만 런타임 에러가 발생하게 된다.

```ts
const people = ["alice", "bob", "jan"].map((name) => ({} as Person)); // 오류 없음
```

name의 타입과 반환하는 타입을 직접 명시해서 해결

```ts
const people: Person[] = ["alice", "bob", "jan"].map(
  (name): Person => ({ name })
);
```

타입스크립트보다 타입에 대해서 더 잘알고 있을때에는 타입 단언문과 null 아님 단언문을 작성할 수 있다.

```ts
document.querySelector("#myButton").addEventListener("click", (e) => {
  e.currentTarget; // 타입은 EventTarget
  const button = e.currentTarget as HTMLButtonElement;
  button; // 타입은 HTMLButtonElement
});
```

타입 스크립트 Dom에 접근할 수 없기 때문에 버튼 엘리먼트를 알지 못한다.

우리는 타입스크립트가 알지 못하는 정보를 가지고 있기때문에 여기서는 타입단언문을 사용하는것이 타당하다.

(!)을 사용해서 null이 아님을 단언할 수 있다.

```ts
const elNull = document.getElementById("foo"); // 타입 HTMLElement | null
const el = document.getElementById("foo")!; // 타입 HTMLElement
```
