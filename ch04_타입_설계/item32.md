# 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

유니온의 인터페이스보다 인터페이스의 유니온이 더 정확하고 타입스크립트가 이해하기도 좋습니다.

```ts
// Bad
interface Layer {
  layout: FillLayout | LineLayout | PointLayout;
  paint: FillPaint | LinePaint | PointPaint;
}

// Good
interface FillLayer {
  layout: FillLayout;
  paint: FillPaint;
}
interface LineLayer {
  layout: LineLayout;
  paint: LinePaint;
}
interface PointLayer {
  layout: PointLayout;
  paint: PointPaint;
}
type Layer = FillLayer | LineLayer | PointLayer;
```

타입 정보를 담고 있는 주석은 문제가 될 소지가 매우 높습니다.(직접 수정하지 않는 이상 자동 동기화가 되지 않음)
두 개의 속성을 하나의 객체로 모으는 것이 더 나은 설계입니다.

```ts
// Bad
interface Person {
  name: string;
  // 다음은 둘 다 동시에 있거나 동시에 없습니다.
  placeOfBirth?: string;
  dateOfBirth?: Date;
}

// Good
interface Person {
  name: string;
  birth?: {
    place: string;
    date: Date;
  };
}
```

타입의 구조를 손 댈 수 없는 상황(예를 들어 API의 결과)이면, 앞서 다룬 인터페이스의 유니온을 사용해서 속성 사이의 관계를 모델링할 수 있습니다.

```tsx
interface Name {
  name: string;
}

interface PersonWithBirth extends Name {
  placeOfBirth: string;
  dateOfBirth: Date;
}

type Person = Name | PersonWithBirth;
```
