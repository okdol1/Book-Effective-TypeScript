# 타입과 인터페이스의 차이점 알기

```ts
// type

type TState = {
  name: string;
  capital: string;
};

// interface

interface IState {
  name: string;
  capital: string;
}
```

## 공통점

- 추가 속성과 함께 할당한다면 오류가 발생한다
- 인덱스 시그니처를 사용할 수 있다.

```ts
type Tdict = { [key: string]: string };
interface IDict {
  [key: string]: string;
}
```

- 함수 타입을 정의할 수 있다.

```ts
type TFn = (x: number) => string;
interface IFn {
  (x: number) => string;
}
```

- 제네릭이 가능하다.

```ts
type TPair<T> = {
  first: T;
  second: T;
};
interface IPair<T> {
  first: T;
  second: T;
}
```

- 인터페이스는 타입을 확장할 수 있으며, 타입은 인터페이스를 확장할 수 있다.

```ts
interface IStateWithPop extends TState {
  population: number;
}
type TStateWithPop = IState & { population: number };
```

- IStateWithPop과 TStateWithPop은 동일하다.

> 인터페이스는 유니온 타입 같은 복잡한 타입을 확장하지 못한다. 복잡한 타입을 확장하고 싶으면 타입과 &를 사용해야 한다.

- 클래스를 구현할 수 있다.

```ts
class StateT implements TState {
  name: string = "";
  capital: string = "";
}
class StateI implements IState {
  name: string = "";
  capital: string = "";
}
```

## 차이점

- 튜플구현

```ts
type Pair = [number, number];

interface Tuple {
  0: number;
  1: number;
  length: 2;
}
```

> 인터페이스로 튜플과 비슷하게 구현하면 튜플에서 사용할 수 있는 concat 과 같은 메서드를 사용할 수 없다. 따라서 튜플은 type 키워드로 구현하자

- 인터페이스는 선언 병합이 가능하다.

```ts
interface IState {
  name: string;
}
interface IState {
  capital: string;
}
const ex: IState = {
  name: 'yongwoo';
  capital: 'seoul'
}; // 정상
```

> 타입 선언 파일을 작성할 때는 선언 병합을 지원하기 위해 반드시 인터페이스를 사용해야 하며 표준을 따라야 한다.

복잡한 타입이라면 고민할 것도 없이 타입 별칭을 사용한다.

그러나 타입과 인터페이스, 두 가지 방법으로 모두 표현할 수 있는 간단한 객체 타입이라면 일관성과 보강의 관점에서 고려해 봐야 한다.
