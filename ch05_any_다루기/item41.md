# any의 진화를 이해하기

- any 타입의 진화는 noImplicitAny가 설정된 상태에서 변수의 타입이 암시적 any인 경우에만 일어난다.

```ts
// 배열 예시
function range(start: number, limit: number) {
  const out = []; // 타입이 any[]
  for (let i = start; i < limit; i++) {
    out.push(i); // out의 타입이 any[]
  }
  return out; // 타입이 number[]
}

// 단순값 예시
let val; // 타입이 any
if (Math.random()<0.5>) {
  val = /hello/;
  val; // 타입이 RegExp
} else {
  val = 12;
  val; // 타입이 number
}
val; // 타입이 number | RegExp
```

- 타입의 진화는 타입 좁히기([아이템 22](https://github.com/okdol1/Book-Effective-TypeScript/blob/main/ch03_%ED%83%80%EC%9E%85_%EC%B6%94%EB%A1%A0/item22.md))와 다르다. 배열에 다양한 타입의 요소를 넣으면 배열의 타입이 확장되며 진화한다.

```ts
const result = []; // 타입이 any[]
result.push("a");
result; // 타입이 string[]
result.push(1);
result; // 타입이 (string | number)[]
```

- any타입의 진화는 암시적 any 타입에 어떤 값을 할당할 때만 발생한다. 그리고 어떤 변수가 암시적 any 상태일 때 값을 읽으려고 하면 오류가 발생한다.

```ts
function range(start: number, limit: number) {
  const out = [];
  //  ~~~ 'out' 변수는 형식을 확인할 수 없는 경우 일부 위치에서 암시적으로 'any[]' 형식입니다.
  if (start === limit) {
    return out;
    //  ~~~ 'out' 변수에는 암시적으로 'any[]' 형식이 포함됩니다.
  }
  for (let i = start; i < limit; i++) {
    out.push(i);
  }
  return out;
}
```

- any를 진화시키는 방식보다 명시적 타입 구문을 사용하는 것이 안전한 타입을 유지하는 방법이다.
