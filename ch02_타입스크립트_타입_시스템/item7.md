# 타입이 값들의 집합이라고 생각하기

# never

가장 작은 집합은 아무 값도 포함하지 않는 공집합, 타입스크립트에서는 never 타입입니다.

never타입으로 선언된 변수의 범위는 공집합이기 때문에 아무런 값도 할당할 수 없습니다.

# 리터럴(literal)타입

다음으로 작은 집합은 한 가지 값만 포함하는 타입

타입스크립트에서 유닛(unit)타입이라고도 불리는 리터럴(literal) 타입입니다.

```ts
type A = "A";
type b = "B";
type Twelve = 12;
```

# 유니온(union)타입

두개 혹은 세 개로 묶은 타입

> 유니온 타입은 값 집합들의 ‘합집합’을 일컫습니다.

```ts
type AB = "A" | "B";
type AB12 = "A" | "B" | 12;

const a: AB = "A"; // 정상
const c: AB = "C"; // 'C'형식은 AB형식에 할당할 수 없습니다.
```

### 타입체커

집합의 관점에서 타입 체커의 주요역할은 하나의 집합이 다른 집합의 부분 집합인지 검사하는 것이라고 볼 수 있다.

```ts
type AB = "A" | "B";
type AB12 = "A" | "B" | 12;
// OK, {"A", "B"} is a subset of {"A", "B"}:
const ab: AB = Math.random() < 0.5 ? "A" : "B";
const ab12: AB12 = ab; // OK, {"A", "B"} 는 {"A", "B", 12} 의 부분 집합

declare let twelve: AB12;
const back: AB = twelve;
// ~~~~ Type 'AB12' 는 'AB' 형식에 할당할 수 없습니다.
//        Type '12' 는 'AB' 형식에 할당할 수 없습니다
```

### 원소를 서술하는 방법

```ts
type Int = 1 | 2 | 3 | 4 | 5 | ...
```

# 인터섹션 타입

> &연산자는 두 타입의 인터섹션(intersection, 교집합)을 계산합니다.

```ts
interface Person {
  name: string;
}
interface Lifespan {
  birth: Date;
  death?: Date;
}
type PersonSpan = Person & Lifespan;

const ps: PersonSpan = {
  name: "Alan Turing",
  birth: new Date("1912/06/23"),
  death: new Date("1954/06/07"),
}; // 정상
```

타입 연산자는 인터페이스의 속성이 아닌, 값의 집합(타입의 범위)에 적용됩니다.

그리고 추가적인 속성을 가지는 값도 여전히 그 타입에 속합니다. 그래서 Person과 Lifespan을 둘다 가지는 값은 인터섹션 타입에 속합니다.

당연히 앞의 세가지(name, birth, death)보다 더 많은 속성을 가지는 값도 PersonSpan타입에 속합니다.

인터섹션 타입의 값은 각 타입 내의 속성을 모두 포함하는 것이 일반적인 규칙

```ts
type K = keyof (Person | Lifespan); // 타입이 never
```

유니온 타입에 속하는 값은 어떠한 키도 없기 때문에, 유니온에 대한 keyof는 공집합입니다.

```ts
keyof (A&B) = (keyof A) | (keyof B)
keyof (A|B) = (keyof A) & (keyof B) // A와 B 겹치는 것이 없으니 never
```

# extend

```ts
interface Person {
  name: string;
}
interface PersonSpan extends Person {
  birth: Date;
  death?: Date;
}
```

> 타입이 집합이라는 관점에서 Extends의 의미는 ‘~에 할당 가능한’과 비슷하게 ‘~의 부분 집합’이라는 의미로 받아들일 수 있습니다.

PersonSpan 타입의 모든 값은 문자열 name 속성을 가져야 합니다. 그리고 birth 속성을 가져야 제대로 된 부분 집합이 됩니다.

```ts
interface Vector1D {
  x: number;
}
interface Vector2D extends Vector1D {
  y: number;
}
interface Vector3D extends Vector2D {
  z: number;
}

// 위 코드를 extends 없이 작성하면 이렇게
interface Vector1D {
  x: number;
}
interface Vector2D {
  x: number;
  y: number;
}
interface Vector3D {
  x: number;
  y: number;
  z: number;
}
```

위 코드의 관계를 그림으로 나타내면 아래와 같습니다. extends없이 인터페이스로 코드를 재작성해 보면
부분집합, 서브타입, 할당 가능성의 관계가 바뀌지 않는다는 걸 명확히 알 수 있습니다.
