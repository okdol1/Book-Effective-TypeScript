# 구조적 타이핑에 익숙해지기

자바스크립트는 본질적으로 덕 타이핑(duck typing) 기반이다.

### 덕 타이핑이란?

- 객체의 변수 및 메서드 집합이 타입을 결정

  > 즉, 객체가 어떡 타입에 부합하는 변수와 메서드를 가질 경우, 객체를 해당 타입에 속하는 것으로 간주하는 것

- 많은 새들 중에서 오리처럼 걷고, 헤엄치고, 꽥꽥거리면 그 새를 오리라고 할 수 있는 것에서 유래

### 구조적 타이핑(structural typing)

- 타입 구조가 유사하면(ex. 객체의 프로퍼티들이 비슷) 다른 두 타입이 서로 호환될 수 있는 것
- JS가 덕 타이핑 기반이고, TS는 JS의 런타임 동작을 모델링하기 때문에, 구조적 타이핑 발생

```ts
interface Vector2D {
  x: number;
  y: number;
}

// 함수 선언시 매개변수를 Vector2D 타입으로 설정
function calculateLength(v: Vector2D) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

interface NamedVector {
  name: string;
  x: number;
  y: number;
}

const v: NamedVector = {
  x: 3,
  y: 4,
  name: "Zee",
};

console.log(calculateLength(v)); // 5
```

Vector2D 타입과 NamedVetor 간 관계를 선언하지 않았는데 호환이 가능하다

- NamedVector 타입의 매개변수도 문제없이 사용 가능하다
- 별도의 caculateLength 함수를 정의할 필요가 없다

이는 타입스크립트 타입이 open 되어 있다는 것을 잘 보여줌(타입의 확장이 열려 있다)

타입에 선언된 속성 이외에 임의의 속성을 추가하더라고 오류 발생 X

### 구조적 타이핑으로 인한 문제

```ts
interface Vector3D {
  x: number;
  y: number;
  z: number;
}

function calculateLength1(v: Vector3D) {
  let length = 0;
  for (const axis of Object.keys(v)) {
    const coor = v[axis];
    // ~~~~ 'string'은 'Vector3D'의 인덱스로 사용할 수 없기에 엘리먼트는 암시적으로 'any' 타입입니다.
    length += Math.abs(coord);
  }
  return length;
}
```

axis는 Vector3D 타입인 v의 키 중 하나이기 때문에 "x", "y", "z" 중 하나여야 한다. 그리고 Vector3D의 선언에 따르면, 이들은 모두 number이므로 coord의 타입이 number가 되어야할 것으로 예상한다.

그러나 타입스크립트가 오류를 정확히 찾아낸 것이 맞다.

```ts
interface Vector3D {
  x: number;
  y: number;
  z: number;
}

function calculateLength1(v: Vector3D) {
  let length = 0;
  for (const axis of Object.keys(v)) {
    const coor = v[axis];
    // ~~~~ 'string'은 'Vector3D'의 인덱스로 사용할 수 없기에 엘리먼트는 암시적으로 'any' 타입입니다.
    length += Math.abs(coord);
  }
  return length;
}

const example = {
  x: 3,
  y: 4,
  z: 5,
  text: "텍스트입니다",
};

calculateLength1(example); // 타입에러 발생하지 않음
```

example에는 `text: string;`이 추가로 있지만 Vector3D가 되어야하는 calculateLength1의 매개변수가 될 수 있음 -> 덕 타이핑의 문제

- v 속성은 어떤 속성이든 가질 수 있기 때문에, axis 타입은 string이 될 수도 있음
- 정확한 타입으로 객체를 순회하는 것은 까다로움

위 코드의 해결방법으로 루프보다는 모든 속성을 각각 더하는 방법으로 구현하는 것이 좋다.

```ts
function calculateLength1(v: Vector3D) {
  return Math.abs(v.x) + Math.abs(v.y) + Math.abs(v.z);
}
```
