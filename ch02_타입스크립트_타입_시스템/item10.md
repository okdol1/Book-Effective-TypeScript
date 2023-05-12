# 객체 래퍼 타입 피하기

자바스크립트는 객체 외에도 기본 형 값들에 대한 일곱가지 타입이 있다.

- string
- number
- boolean
- null
- undefined
- symbol (ES2015 추가)
- bigint

기본형은 불변이며, 메서드를 가지지 않는다는 점에서 객체와 구분된다.

```ts
"primitive".charAt(3);
```

하지만 , 위와 같은 예제는 메서드를 가지고 있는 것처럼 보이게 된다. string 기본형에는 없지만 String 객체 타입에는 메서드를 가지고 있기 때문이다.

자바스크립트의 동작

> string(기본형) => String(객체)로 매핑 => 메서드 호출

그러나, string 타입을 String(객체)로 사용하면 다른 타입이라고 인지하기 때문에 주의해야 한다.
