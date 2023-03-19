# 함수 안으로 타입 단언문 감추기

타입 선언문은 일반적으로 타입을 위험하게 만들지만 상황에 따라 현실적인 해결책이 되기도 한다. 불가피하게 사용해야 한다면, 함수 내부에는 타입 단언을 사용하고 함수 외부로 드러나는 타입 정의를 정확히 명시하는 정도로 끝내는게 낫다.

```ts
function cacheLast<T extends Function>(fn: T): T {
  let lastArgs: any[] | null = null;
  let lastResult: any;
  return function (...args: any[]) {
    if (!lastArgs || !shallowEqual(lastArgs, args)) {
      lastResult = fn(...args);
      lastArgs = args;
    }
    return lastResult;
  } as unknown as T;
}
```
