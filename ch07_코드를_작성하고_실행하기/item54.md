# 객체를 순회하는 노하우

아래 코드는 정상적으로 실행되지만, 편집기에서는 오류가 발생한다.

```ts
const obj = {
  one: "uno",
  two: "dos",
  three: "tres",
};
for (const k in obj) {
  const v = obj[k]; // ~~~ obj에 인덱스 시그니처가 없기 때문에 엘리먼트는 암시적으로 'any' 타입입니다.
}
```

k와 obj 객체의 키 타입이 서로 다르게 추론되어 오류가 발생.

k의 타입을 더욱 구체적으로 명시해 주면 오류가 사라진다.

```ts
let k: keyof typeof obj; // "one" | "two" | "three" 타입
for (k in obj) {
  const v = obj[k]; // 정상
}
```

그러나 keyof 키워드를 사용한 방법은 또 다은 문제점을 내포하고 있다.

- k가 "one" | "two" | "three" 타입으로 한정됨
- v도 string 타입으로 한정되어 범위가 좁아짐

> 단지 객체의 키와 값을 순회하고 싶다면 Object.entries를 사용하자

```ts
interface ABC {
  a: string;
  b: string;
  c: number;
}

function foo(abc: ABC) {
  for (const [k, v] of Object.entries(abc)) {
    k; // string 타입
    v; // any 타입
  }
}
```
