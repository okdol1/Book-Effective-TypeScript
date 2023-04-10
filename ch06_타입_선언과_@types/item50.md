# 오버로딩 타입보다는 조건부 타입을 사용하기

> 오버로딩 타입보다는 조건부 타입을 사용하는 것이 좋다. 조건부 타입은 추가적인 오버로딩 없이 유니온 타입을 지원 가능하기 때문이다.

타입스크립트는 오버로딩 타입 증에서 일치하는 타입을 찾을 때까지 순차적으로 검색한다.

```ts
// Bad
{
  function double(x: number | string): number | string;
  function double(x: any) {
    return x + x;
  }

  const num = double(2); // type: string | number
  const str = double("x"); // type: string | number
  //선언이 틀리진 않았지만 모호하다.
}

{
  function double<T extends number | string>(x: T): T;
  function double(x: any) {
    return x + x;
  }

  const num = double(2); // type: 2
  const str = double("x"); // type: 'x'
  //타입이 과하게 구체적이다.
}

{
  function double(x: number): number;
  function double(x: string): string;
  function double(x: any) {
    return x + x;
  }

  const num = double(2); // type: number
  const str = double("x"); // type: string
  //타입이 명확해졌지만 버그가 발생한다.

  function f(x: number | string) {
    return double(x); //'string|number' 형식의 인수는 'string'형식의 매개변수에 할당될 수 없습니다.
  }
}

// Good
function double<T extends number | string>(
  x: T
): T extends string ? string : number;
function double(x: any) {
  return x + x;
}

const num = double(12); //number
const str = double("x"); //string

function f(x: number | string) {
  return double(x);
}
```
