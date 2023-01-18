# 타입 연산과 제너릭 시용으로 반복 줄이기

> Don't repeat yourself(DRY)

## 요약

- DRY(don't repeat yourself) 원칙을 타입에도 최대한 적용해야 합니다.
- 타입에 이름을 붙여서 반복을 피해야 합니다. extends를 사용해서 인터페이스 필드의 반복을 피해야 합니다.
- 타입들 간의 매핑을 위해 타입스크립트가 제공한 도구들을 공부하면 좋습니다. 여기에는 keyof, typeof, 인덱싱, 매핑된 타입들이 포함됩니다.
- 제너릭 타입은 타입을 위한 함수와 같습니다. 타입을 반복하는 대신 제너릭 타입을 사용하여 타입들 간에 매핑을 하는 것이 좋습니다. 제너릭 타입을 제한하려면 extends를 사용하면 됩니다.
- 표준 라이브러리에 정의된 Pick, Partial, ReturnType 같은 제너릭 타입에 익숙해져야 합니다.

### 기존의 중복 제거

```ts
interface Point2D {
  x: number;
  y: number;
}
function distance(a: Point2D, b: Point2D) {
  /* ... */
}
```

```ts
type HTTPFunction = (url: string, options: Options) => Promise<Response>;
const get: HTTPFunction = (url, options) => {
  return Promise.resolve(new Response());
};
const post: HTTPFunction = (url, options) => {
  return Promise.resolve(new Response());
};
```

```ts
interface Person {
  firstName: string;
  lastName: string;
}

interface PersonWithBirthDate extends Person {
  birth: Date;
}
```

명명된 타입, 함수 시그니처 분리, 확장 등 기본적인 문법으로 중복을 제거할 수 있습니다.

### 객체의 일부 속성만 포함하는 타입

```ts
interface State {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
  pageContents: string;
}
// Bad
interface TopNavState {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
}
// Good
type TopNavState = {
  [k in "userId" | "pageTitle" | "recentFiles"]: State[k];
};
// Best
type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
```

### 태그된 유니온의 태그 타입

```ts
interface SaveAction {
  type: "save";
  // ...
}
interface LoadAction {
  type: "load";
  // ...
}
type Action = SaveAction | LoadAction;
// Bad
type ActionType = "save" | "load";
// Good
type ActionType = Action["type"];
```

### 객체 내 모든 속성이 optional한 타입

```ts
interface Options {
  width: number;
  height: number;
  color: string;
  label: string;
}
// Bad
interface OptionsUpdate {
  width?: number;
  height?: number;
  color?: string;
  label?: string;
}
// Good
type OptionsUpdate = { [k in keyof Options]?: Options[k] };
// Best
type OptionsUpdate = Partial<Options>;
```

### 객체 값 타입

```ts
const INIT_OPTIONS = {
  width: 640,
  height: 480,
  color: "#00FF00",
  label: "VGA",
};
// Bad
interface Options {
  width: number;
  height: number;
  color: string;
  label: string;
}
// Good
type Options = typeof INIT_OPTIONS;
```

‘타입 공간’의 typeof 를 사용하여 정의된 객체의 값 형태에 해당하는 타입을 추출할 수 있습니다.

### 함수 리턴 타입

```ts
function getUserInfo(userId: string) {
  // ...

  return {
    userId,
    name,
    age,
    height,
    weight,
    favoriteColor,
  };
}
type UserInfo = ReturnType<typeof getUserInfo>;
```

ReturnType은 함수인 getUserInfo가 아니라 타입인 typeof getUserInfo에 적용되었습니다.

### 제네릭 매개변수 제한

```ts
interface Name {
  first: string;
  last: string;
}
type DancingDuo<T extends Name> = [T, T];

const couple1: DancingDuo<Name> = [
  { fist: "Fred", last: "Astaire" },
  { first: "Ginger", last: "Rogers" },
]; // OK
const couple2: DancingDuo<{ first: string }> = [
  // 오류
  { first: "Sonny" },
  { first: "Cher" },
];
```

> Type ‘{ first: string; }’ does not satisfy the constraint ‘Name

extends로 매개변수에 전달 될 수 있는 타입을 제한할 수 있습니다.

```ts
type Pick<T, K extends keyof T> = {
  [k in K]: T[k];
};
type FirstMiddle = Pick<Name, "first" | "middle">; // 오류
```

Pick 타입의 두번째 파라미터로 전달될 수 있는 타입은 keyof T 타입으로 제한됩니다.
