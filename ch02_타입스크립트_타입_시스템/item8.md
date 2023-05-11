# 타입 공간과 값 공간의 심벌 구분하기

심벌은 이름이 같더라도 속하는 공간에 따라 다른 것을 나타낼 수 있기 때문에 혼란스러울 수 있다.

```ts
interface Cylinder {
  // 타입
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height }); // 값

function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape.radius;
    // ~~~~~~ {} 형식에 radius 속성이 없습니다.'
  }
}
```

interface Cylinder의 Cylinder는 타입으로 쓰인다. const Cylinder에서 Cylinder와 이름은 같지만 값으로 쓰인다.

이 두 개는 서로 아무런 관련도 없습니다. 상황에 따라서 타입으로 쓰일 수도 있고, 값으로 쓰일 수도 있다.

이런 점이 가끔 오류를 발생시킨다.

instanceof를 이용해 shape가 Cylinder타입인지 체크하려고 했는데
instanceof는 자바스크립트의 런타임 연산자이고, 값에 대해서 연산을 한다.

그래서 instanceof Cylinder는 타입이 아니라 함수를 참조한다.

```ts
type, interface 다음에 나오는 심벌은 ‘타입’, const, let 선언에 쓰이는 것은 ‘값’
type T1 = 'string literal';   // 타입
type T2 = 123;                // 타입
const v1 = 'string literal';  // 값
const v2 = 123;               // 값
```

자바스크립트 코드 컴파일 과정에서 타입 정보는 제거되기 때문에, 심벌이 사라진다면 그것은 타입에 해당된다.

T1, T2는 사라지고 v1,v2만 남아있게된다.

```ts
const v1 = "string literal"; // 값
const v2 = 123; // 값
```

타입선언(`:`), 단언문(`as`) 다음에 나오는 심벌은 ‘타입’, `=` 다음에 나오는 모든 것은 ‘값’

```ts
interface Person {
  first: string;
  last: string;
}
const p: Person = { first: "Jane", last: "Jacobs" };
//    -           --------------------------------- 값
//       ------ 타입
```

두 공간 사이에서 다른 의미를 가지는 코드 패턴

|           | 값                    | 타입                                                                   |
| --------- | --------------------- | ---------------------------------------------------------------------- |
| `this`    | 자바스크립트의 this   | 다형성(polymorphic) this 로 서브 클래스의 메서드 체인을 구현할 때 유용 |
| `&`와`ㅣ` | AND 와 OR 비트 연산   | 인터섹션과 유니온                                                      |
| `const`   | 새 변수 선언          | as const는 리터럴 또는 리터럴 표현식의 추론된 타입을 바꿈              |
| `extends` | class에서 상속할 때   | 서브 클래스 또는 서브타입 또는 제너릭 타입의 한정자                    |
| `in`      | for in, key in object | 루프 또는 매핑된 타입                                                  |
