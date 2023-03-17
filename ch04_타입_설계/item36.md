# 해당 분야의 용어로 타입 이름 짓기

가독성을 높이고, 추상화 수준을 올리기 위해서 해당 분야의 용어를 사용해야 합니다.

```ts
// Bad
interface Animal {
  name: stringl
  endangered: boolean;
  habitat: string;
}

const leaopard: Animal {
  name: 'Snow Leopard',
  endangered: false,
  habitat: 'tundra',
}
```

이 코드에는 네 가지 문제가 있다.

- name은 매우 일반적인 용어이므로 의미가 불분명하다
- endangered는 이미 멸종된 동물일 경우 어떻게 표현할지 불분명하다
- habit이 단순 string이라 범위가 너무 넓다
- 객체의 명과 name이 겹쳐서 의도가 다른지 불분명하다

조금 더 명확하게 데이터를 표현하고자 한다면 다음과 같을것이다

```ts
// Good
interface Animal {
  commonName: string;
  genus: string;
  species: string;
  status: ConservationStatus;
  climates: KoppenClimate[];
}
type ConservationStatus = "EX" | "EW" | "CR";
type KoppenClimate = "Af" | "Am" | "As" | "Aw";

const snowLeopard: Animal = {
  commonName: "Snow LeoPard",
  genus: "Panthera",
  species: "Uncia",
  status: "EW",
  climates: ["As", "Aw"],
};
```

- 동일한 의미를 나타낼 때에는 같은 용어를 사용해야한다
- data, item, object 등 모호하고 의미없는 이름은 지양한다
- 포함된 내용, 계산 방식 등이 아닌 데이터 자체가 무엇인지 고려해야한다
