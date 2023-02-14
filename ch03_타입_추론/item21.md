# 타입 넓히기

> 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추해야하는 과정을 ‘넓히기(widening)’라고 부른다.

```jsx
interface Vetor3 {
  x: number;
  y: number;
  z: number;
}
function getComponent(vector: Vector3, axis: "x" | "y" | "z") {
  return vector[axis];
}

let x = "x";
let vec = { x: 10, y: 20, z: 30 };
getComponent(vex, x); // Error: ~ 'string' 형식의 인수는 '"x" | "y" | "z"'
//현식의 매개변수에 할당될 수 없습니다.
```

- getComponent 함수는 두 번째 매개변수에 “x” | “y” | “z” 타입을 기대했지만, x의 타입을 할당 시점에 넓히기가 동작해서 string으로 추론되었다.
- 타입 넓히기가 진행될 때, 정보가 충분하지 않다면 주어진 값으로 추론 가능한 타입이 여러개이기 때문에 과정히 살당히 모호해진다.

타입스크립트의 기본 동작을 재정의하는 세 가지 방법

1. 명시적 타입 구문을 제공하는 것

```jsx
const v: { x: 1 | 3 | 5 } = {
  x: 1,
}; // 타입이 {x: 1|3|4;}
```

1. 타입 체커의 추가적인 문맥을 제공하는 것(예를 들어, 함수의 매개변수로 값을 전달)
2. const 단언문을 사용하는 것

```jsx
const v1 = {
	x: 1,
	y: 2,
}; // 타입은 {x: number; y: number;}

const v2 {
	x: 1 as const,
	y: 2,
}; // 타입은 {x: 1, y: number;}

const v3 {
	x: 1,
	y: 2,
} as const; // 타입은 {readonly x: 1; readonly y: 2;}
```

- 값 뒤에 as const 를 작성하면, 타입스크립트는 최대한 좁은 타입으로 추론한다
- v3에는 넓히기가 동작하지 않는다
