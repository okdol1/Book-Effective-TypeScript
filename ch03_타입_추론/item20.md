# 다른 타입에는 다른 변수 사용하기

> 변수의 값은 바뀔 수 있지만 그 타입은 보통 바뀌지 않는다.

```ts
// Bad
let id: string | number = '12-34-56';
fetchProduct(id);

id = 123456;
fetchProductBySerialNumber(id);

// Good
const id = '12-34-56';
fetchProduct(id);

const serial = 123456;
fetchProductBySerialNumber(serial);
```
다른 타입에는 별도의 변수를 사용하는 것이 좋습니다.

- 서로관련없는 두 값을 분리합니다. (id와 serial)
- 변수명을 구체적으로 지을 수 있습니다.
- 타입 추론을 향상시키고, 타입 구문이 불필요해집니다
- let 대신 const를 사용할 수 있습니다.