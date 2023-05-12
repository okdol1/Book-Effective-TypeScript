# 잉여 속성 체크의 한계 인지하기

- 객체 리터럴을 변수에 할당하거나 함수에 매개변수로 전달할 때 잉여 속성 체크가 수행된다.
- 잉여 속성 체크는 오류를 찾는 효과적인 방법이지만, 타입스크립트 타입 체커가 수행하는 일반적인 구조적 할당 가능성 체크와 역할이 다르다.
- 할당의 개념을 정확히 알아야 잉여 속성 체크와 일반적인 구조적 할당 가능성 체크를 구분할 수 있다.

```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}

// 잉여 속성 체크 수행
const r1: Room = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present",
  // ~~~ 개체 리터럴은 알려진 속성만 지절할 수 있으며 'Room' 형식에 'elephant'이(가) 없습니다.
};

// 구조적 할당 체크
const obj = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present",
};

const r2: Room = obj; //정상
```

잉여 속성 체크에는 한계가 있다. 임시 변수를 도입하면 잉여 속성 체크를 건너뛸 수 있다.
