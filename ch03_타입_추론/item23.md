# 한꺼번에 객체 생성하기

### 한꺼번에 객체 생성하기

```ts
// Bad 1

const pt = {};
pt.x = 3;
// ~ Property 'x' does not exist on type '{}'
pt.y = 4;
// ~ Property 'y' does not exist on type '{}'

// Bad 2

interface Point {
  x: number;
  y: number;
}
const pt: Point = {};
// ~~ Type '{}' is missing the following properties from type 'Point': x, y
pt.x = 3;
pt.y = 4;

// Bad 3

const pt = {} as Point;
pt.x = 3;
pt.y = 4; // OK

// Good

const pt: Point = {
  x: 3,
  y: 4,
};
```

### 작은 객체들을 조합해서 큰 객체를 만들어야 할 때 객체 전개 연산자 `...` 사용하기

```ts
// Bad

interface Point {
  x: number;
  y: number;
}
const pt = { x: 3, y: 4 };
const id = { name: "Pythagoras" };
const namedPoint = {};
Object.assign(namedPoint, pt, id);
namedPoint.name;
// ~~~~ Property 'name' does not exist on type '{}'

// Good

const namedPoint = { ...pt, ...id };
namedPoint.name; // OK, type is string
```

### 타입에 안전한 방식으로 조건부 속성 추가하기

```ts
declare let hasMiddle: boolean;
const firstLast = { first: "Harry", last: "Truman" };
const president = { ...firstLast, ...(hasMiddle ? { middle: "S" } : {}) };
```

- 속성을 추가하지 않는 `null` 또는 `{}`으로 객체 전개를 사용
- `president` 타입 추론에서 해당 타입이 선택적 속성을 가진것으로 추론됨

```ts
const president: {
  middle?: string;
  first: string;
  last: string;
};
```

## 요약

- 속성을 제각각 추가하지 말고 한꺼번에 객체로 만들어야 합니다. 안전한 타입으로 속성을 추가하려면 객체 전개 `({...a, ...b})`를 사용하면 됩니다.
- 객체에 조건부로 속성을 추가하는 방법을 익히도록 합니다.
