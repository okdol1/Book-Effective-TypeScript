# 동적 데이터에 인덱스 시그니처 사용하기

## 요약

- 런타임 때까지 객체의 속성을 알 수 없을 경우에만(예를 들어 CSV 파일에서 로드하는 경우) 인덱스 시그니처를 사용하도록 합니다.
- 안전한 접근을 위해 인덱스 시그니처의 값 타입에 undefined를 추가하는 것을 고려해야 합니다.
- 가능하다면 인터페이스, Record, 매핑된 타입 같은 인덱스 시그니처보다 정확한 타입을 사용하는 것이 좋습니다.

### 인덱스 시그니처를 명시하여 유연하게 매핑 표현하기

```ts
type Rocket = { [property: string]: string };
const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,940 kN",
}; // 정상
```

`[property: string]: string`이 인덱스 시그니처이며, 다음 세 가지 의미를 담고 있습니다.

- 키의 이름: 키의 위치만 표시하는 용도입니다. 타입 체커에서는 사용하지 않기 때문에 무시할 수 있는 참고 정보라고 생각해도 됩니다.
- 키의 타입: 보통은 string을 사용합니다.
- 값의 타입: 어떤 것이든 될 수 있습니다.

이렇게 타입 체크가 수행되면 네 가지 단점이 드러납니다.

- 잘못된 키를 포함해도 인식할 수 없습니다. (name vs Name)
- 빈 객체 ({})도 할당될 수 있습니다.
- 다른 타입의 값을 가질 수 없습니다. (number, boolean 등)
- 자동완성 등의 언어서비스를 사용할 수 없습니다.

### 인덱스 시그니처보다 정확한 타입을 사용하기

```ts
// Bad 너무 광범위
interface Row1 {
  [column: string]: number;
}
// Good 최선
interface Row2 {
  a: number;
  b?: number;
  c?: number;
  d?: number;
}
// Good 가장 정확하지만 사용하기 번거로움
type Row3 =
  | { a: number }
  | { a: number; b: number }
  | { a: number; b: number; c: number }
  | { a: number; b: number; c: number; d: number };
```

마지막 형태가 가장 정확하지만, 사용하기에는 조금 번거롭습니다.

string 탕입이 너무 광범위해서 인덱스 시그니처를 사용하는 데 문제가 있다면, 두 가지 다른 대안을 생각해 볼 수 있습니다.

1. Record를 사용하는 방법
1. 매핑된 타입을 사용하는 방법

```ts
type Vec3D = Record<"x" | "y" | "z", number>;
type Vec3D = { [k in "x" | "y" | "z"]: number };
type ABC = { [k in "a" | "b" | "c"]: k extends "b" ? string : number };
```
