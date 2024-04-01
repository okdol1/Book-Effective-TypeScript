# 매핑된 타입을 사용하여 값을 동기화하기

```ts
interface ScatterProps {
  // The data
  xs: number[];
  ys: number[];

  // Display
  xRange: [number, number];
  yRange: [number, number];
  color: string;

  // Events
  onClick: (x: number, y: number, index: number) => void;
}

function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k]) {
      if (k !== "onClick") return true;
    }
  }
  return false;
}
```

서로 관련된 여러 값들이 변할 때, 이들 간의 일관성을 유지하지 않으면 예상치 못한 오류가 발생할 수 있습니다.

만약 `ScatterProps`에 새로운 속성이 추가되면 `shouldUpdate` 함수는 값이 변경될 때마다 이 변경사항을 감지하여 업데이트 여부를 결정합니다. 이는 특히 `onClick`과 같은 함수 타입의 속성을 제외하고, 모든 속성에 대해 이전 값과 새로운 값이 다를 경우에만 true를 반환하여 업데이트가 필요함을 알립니다.

> 위 코드는 새로운 속성이 추가될 때 `shouldUpdate` 함수는 값이 변경될 때마다 차트를 다시 그릴 것이다. 이렇게 처리하는 것을 '보수적(conservative) 접근법' 또는 '실패에 닫힌(fail close)' 접근법이라고 한다.

- 보수적 접근법 (실패에 닫힌 접근법, Fail-Close)
  - 정의: 시스템에 오류가 발생했을 때, 보안이나 무결성을 최우선으로 고려하여 작업을 중지하고 시스템 접근을 차단하는 방식입니다. 이 접근법에서는 시스템이 알려지지 않은 상태나 실패 상태로 진입하는 것을 막기 위해, 기본적으로 모든 작업을 "닫혀 있음" 상태로 처리합니다.
  - 장점: 보안성이 중요한 환경에서는 데이터의 무결성과 시스템의 안정성을 보장하기 위해 필수적입니다. 예상치 못한 오류나 공격 시도로부터 시스템을 보호할 수 있습니다.
  - 단점: 사용자나 시스템의 접근성이 제한될 수 있으며, 서비스 중단이 발생할 가능성이 있습니다. 특히 고가용성이 중요한 서비스에서는 이를 고려한 추가적인 대책이 필요할 수 있습니다.

> 보수적 접근법을 사용하면 차트가 정확하지만 너무 자주 그려질 가능성이 있다. 두 번째 최적화 방법은 '실패에 열린(fail open)'접근법이다.

- 실패에 열린 접근법 (Fail-Open)
  - 정의: 시스템에 오류가 발생했을 때, 접근성이나 사용자의 편의를 최우선으로 고려하여 작업을 계속 진행하고 시스템 접근을 허용하는 방식입니다. 이 접근법에서는 시스템이 실패 상태에도 불구하고 "열려 있음" 상태로 유지됩니다.
  - 장점: 고가용성이 중요한 환경에서 사용자 경험을 해치지 않고 서비스의 지속적인 운영을 보장합니다. 예상치 못한 오류가 발생해도 사용자는 서비스 이용에 큰 지장을 받지 않습니다.
  - 단점: 보안 위험이나 데이터 무결성 문제가 발생할 가능성이 있습니다. 특히 시스템의 보안 구멍을 통해 민감한 정보가 유출될 수 있는 위험이 있으며, 이로 인한 손실이 발생할 수 있습니다.

```ts
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  return (
    oldProps.xs !== newProps.xs ||
    oldProps.ys !== newProps.ys ||
    oldProps.xRange !== newProps.xRange ||
    oldProps.yRange !== newProps.yRange ||
    oldProps.color !== newProps.color
    // (no check for onClick)
  );
}
```

이 코드는 차트를 불필요하게 다시 그리는 단점을 해결했지만, 실제로 차트를 다시 그려야 할 경우 누락되는 일이 생길 수 있다.

> 이는 히포크라테스 전집에 나오는 원칙 중 하나인 '우선, 망치지 말 것(first, fo no ham)'을 어기기 때문에 일반적인 경우에 쓰이는 방법은 아니다.

(참고)

- 보수적 접근법: 방화벽, 인증 시스템 등 보안이 중요한 시스템에서 주로 사용됩니다. 예를 들어, 인증 시스템이 오류를 겪을 경우, 추가적인 접근을 모두 차단하여 보안을 유지할 수 있습니다.
- 실패에 열린 접근법: CDN(Content Delivery Network)이나 캐싱 시스템 등 고가용성이 중요한 서비스에서 주로 사용됩니다. 시스템 오류가 발생해도 사용자가 컨텐츠에 접근할 수 있도록 합니다.

- 위 두가지 방법보다, 타입 체커가 대신할 수 있게 하는 것이 좋다. 핵심은 매핑된 타입과 객체를 사용하는 것이다.

```ts
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
};

function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
      return true;
    }
  }
  return false;
}
```

- `[k in keyof ScatterProps]`은 타입 체커에게 `REQUIRES_UPDATE`가 `ScatterProps`과 동일한 속성을 가져야 한다는 정보를 제공한다.

나중에 `ScatterProps`에 새로운 속성을 추가하는 경우 다음 코드와 같은 형태가 된다.

```ts
interface ScatterProps {
  // ...
  onDoubleClick: () => void;
}
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
  //  ~~~~~~~~~~~~~~~ Property 'onDoubleClick' is missing in type
  // COMPRESS
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
  // END
};
```

이런 방식은 오류를 정확히 잡아 낸다. 속성을 삭제하거나 이름을 바꾸어도 비슷한 오류가 발생한다.

## 요약

- 매핑된 타입을 사용해서 관련된 값과 타입을 동기화하도록 합니다.
- 인터페이스에 새로운 속성을 추가할 때, 선택을 강제하도록 매핑된 타입을 고려해야 합니다.
