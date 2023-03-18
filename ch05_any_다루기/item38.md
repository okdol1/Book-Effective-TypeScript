# any 타입은 가능한 한 좁은 범위에서만 사용하기

함수에서 any 타입 좁게 사용하기

```ts
function processBar(b: Bar) { /* ... */}

function f {
  const x = expressionReturnFoo();
  processBar(x); // Error: 'Foo' 형식의 인수는 'Bar' 형식의 매개변수에 할당될 수 없습니다.
};

// Bad
function f1 {
  const x: any = expressionReturnFoo();
  processBar(x);

  // 만일 x를 반환한다면 문제가 커짐
  return x;
};

// Good
function f2 {
  const x = expressionReturnFoo();
  processBar(x as any);

  return x;
};
```

객체에서 any 타입 좁게 사용학;

```ts
type Foo = { name: string };
type Config = {
  a: number;
  b: number;
  c: { key: Foo };
};

const config1: Config = {
  a: 1,
  b: 2,
  c: {
    key: 123, // Error: number' 형식은 'Foo' 형식에 할당할 수 없습니다.
  },
};

const config2: Config = {
  // a, b의 타입 체크를 하지 않음
  a: true,
  b: "문자",
  c: {
    key: 123, // 에러는 사라졌지만 모든 타입 체크를 무시함
  },
} as any;

const config3: Config = {
  // a, b의 타입 체크는 유효함
  a: 1,
  b: 2,
  c: {
    key: 123 as any,
  },
};
```
