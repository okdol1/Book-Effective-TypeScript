# 모르는 타입의 값에는 any 대신 unknown을 사용하기

unknown은 any 대신 사용할 수 있는 안전한 타입이다.

- 함수의 반환값과 관련된 unknown

```ts
// Bad
interface Book {
  name: string;
  author: string;
}
const book: Book = parseYAML(`
    name: Wuthering Heights
    author: Emily Bronte
`);

alert(book.title); // 오류 없음, 런타임에 "undefined" 경고
book("read"); // 오류 없음, 런타임에 "TypeError: book은 함수가 아닙니다" 예외 발생

// Good
function safeParseYAML(yaml: string): unknown {
  return parseYAML(yaml);
}
const book = safeParseYAML(`
    name: The Tenat of Wildfell Hall
    author: Anne Bronte
`);
alert(book.title);
//  ~~~ 개체가 'unknown' 형식입니다.
book("read");
//  ~~~ 개체가 'unknown' 형식입니다.
```

any가 위험한 이유

1. 어떠한 타입이든 any 타입에 할당 가능하다
1. any 타입은 어떠한 타입으로도 할당 가능하다

unknown 타입은 any의 첫 번째 속성을 만족하지만, 두 번째 속성은 만족하지 않습니다.
반면 never 타입은 unknown 타입과 정반대입니다.

- 변수 선언과 관련된 unknown

```ts
// 어떠한 값이 있지만 그 타입을 모르는 경우
interface Feature {
  id?: string | number;
  geometry: Geometry;
  properties: unknown;
}

// instanceof를 체크한 후 unknown에서 원하는 타입으로 변환
function processValue(val: unknown) {
  if (val instanceof Date) {
    val; // 타입이 Date
  }
}
```

- 단언문과 관련된 unknown

```ts
declare const foo: Foo;

// Bad
let barAny = foo as any as Bar;

// Good
let barUnk = foo as unknown as Bar;
```

- {}, object, unknown 차이점
  - {} 타입은 null과 undefined를 제외한 모든 값을 포함합니다.
  - object 타입은 모든 비기본형(non-primitive) 타입으로 이루어집니다. 여기에는 true 또는 12 또는 "foo"가 포함되지 않지만 객체와 배열은 포함됩니다.
  - unknown 타입이 도입되기 전에는 {}가 더 일반적으로 사용되었지만, 최근에는 {}를 사용하는 경우가 꽤 드뭅니다. 정말로 null과 undefined가 불가능하다고 판단되는 경우만 unknown 대신 {}를 사용하면 됩니다.
