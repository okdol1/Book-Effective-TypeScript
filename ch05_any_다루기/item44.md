# 타입 커버리지를 추적하여 타입 안정성 유지하기

noImplicitAny를 설정하고 모든 암시적 any 대신 명시적 타입 구문을 추가해도 any 타입이 여전히 프로그램 내에 존재할 수 있다.

1. 명시적 any 타입

- [아이템 38](https://github.com/okdol1/Book-Effective-TypeScript/blob/main/ch05_any_%EB%8B%A4%EB%A3%A8%EA%B8%B0/item38.md)과 [아이템 39](https://github.com/okdol1/Book-Effective-TypeScript/blob/main/ch05_any_%EB%8B%A4%EB%A3%A8%EA%B8%B0/item39.md)의 내용에 따라 any 타입의 범위를 좁히고 구체적으로 만들어도 여전히 any이다.
  특히 `any[]`와 `{ [key: string]: any }` 같은 타입은 인덱스를 생성하면 단순 any사 되고 코드 전반에 영향을 미친다.

2. 서드파티 타입 선언

- 이 경우에는 @types 선언 파일로부터 any 타입이 전파되기 때문에 특별히 조심해야 한다.
  noImplicitAny를 설정하고 절대 any를 사용하지 않았다 하더라도 여전히 any 타입 코드 전반에 영향을 미친다.

any 타입은 타입 안정성과 생산성에 부정적 영향을 미칠 수 있으므로 프로젝트에서 any의 개수를 추적하는 것이 좋다.

- npm의 type-coverage 패키지를 활용하여 any를 추적할 수 있다.
- any가 아니거나 any티입이 추가된다면 백분율이 감소하게 된다.
- npx type-coverage --detail 명령어를 사용하면 any 타입이 있는 곳을 모두 출력해준다.
- 이것을 조사해 보면 미처 발견하지 못한 any를 찾을 수 있다.
- noImplicitAny를 설정하고 모든 암시적 any 대신 명시적 타입 구문을 추가해도 any 타입과 관련된 문제들로부터 안전하다고 할 수 없다.
