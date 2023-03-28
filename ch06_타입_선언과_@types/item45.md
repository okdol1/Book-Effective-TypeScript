# devDependencies에 typescript와 @types 추가하기

> npm의 3가지 종류의 의존성

- dependencies: 프로젝트 런타임시 실행되어야할것들 여기에 포함
- devDependencies: 런타임에 필요 없는 라이브러리들 포함
- peerDependencies: 의존성을 직접 관리하지 않는 라이브러리들

ts와 관련 라이브러리들은 일반적으로 devDependencies에 속한다.

> TS 프로젝트에서 공통적으로 고려해야 할 의존성 두 가지

타입스크립트 자체 의존성 고려하기

- 타입스크립트를 시스템 레벨로 설치하는 것을 추천하지 않음
  - 팀원들 모두가 항상 동일한 버전 사용한다는 보장이 없음
  - 프로젝트를 셋업할 때 별도의 단계 추가됨
- npm install을 실행할 때 팀원들 모두 항상 정확한 버전의 타입스크립트를 설치 할 수 있다.
- 타입스크립트를 시스템 레벨로 설치하기보다는 devDependencies에 넣는 것이 좋다.

타입 의존성(@types) 고려하기

- @types 의존성은 devDependencies에 있어야 한다.
- 런타임에 @types가 필요한 경우라면 별도의 작업이 필요하다.

```ts
// 리액트
$ npm install react
$ npm install --save-dev @types/react
```
