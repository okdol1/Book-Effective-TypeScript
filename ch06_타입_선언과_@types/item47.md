# 공개 API에 등장하는 모든 타입을 익스포트하기

서드파티의 모듈에서 익스포트되지 않은 타입 정보가 필요한 경우

```ts
interface SecretName {
  first: string;
  last: string;
}

interface SecretSanta {
  name: SecretName;
  gift: string;
}

export function getGift(name: SecretName, gift: string): SecretSanta {
  // ...
}
```

해당 라이브러리 사용자는 `SecretName` 또는 `SecretSanta`를 직접 임포트할 수 없고, getGift만 임포트 가능하다.

이때 익스포트 되지 않은 타입을 추출하는 한 가지 방법은 `Parameters`와 `ReturnType` 제너릭 타입을 사용한다.

```ts
type MySanta = ReturnType<typeof getGift>; // SecretSanta
type MyName = ReturnType<typeof getGift>[0]; // SecretName
```

| 공개 메서드에 등장한 어떤 형태의 타입이든 익스포트 하자. 어차피 라이브러리 사용자가 추출할 수 있으므로, 익스포트하기 쉽게 만드는 것이 좋다.
