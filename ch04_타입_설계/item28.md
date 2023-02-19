# 유효한 상태만 표현하는 타입을 지향하기

효과적으로 타입을 설계하려면, 유효한 상태만 표현할 수 있는 타입을 만들어 내는 것이 가장 중요합니다.

```ts
// Bad
interface State {
  a: string;
  b: string;
  c?: string;
  d?: string;
}

// Good
interface StateOne {
  a: string;
  b: string;
}

interface StateTwo {
  a: string;
  b: string;
  c: string;
}

interface StateThree {
  a: string;
  b: string;
  d: string;
}

type State = StateOne | StateTwo | StateThree;
```

### 요약

- 유효한 상태와 무효한 상태를 둘 다 표현하는 타입은 혼란을 초래하기 쉽고 오류를 유발하게 됩니다.
- 유효한 상태만 표현하는 타입을 지향해야 합니다. 코드가 길어지거나 표현하기 어렵지만 결국은 시간을 절약하고 고통을 줄일 수 있습니다.
