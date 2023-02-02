# 추론 가능한 타입을 사용해 장황한 코드 방지하기

타입스크립트는 타입 추론을 적극적으로 수행합니다. 타입 추론은 수동으로 명시해야 하는 타입 구문의 수를 엄청나게 줄여 주기 때문에, 코드의 정체적인 안정성이 향상됩니다.

숙련된 타입스크립트 개발자는 비교적 적은 수의 구문(그러나 중요한 부분에는 사용)을 사용합니다. 반면, 초보자의 코드는 불필요한 타입 구문으로 도배되어 있을 겁니다.

다음과 같이 코드의 모든 변수에 타입을 선언하는 것은 비생산적이며 형편없는 스타일로 여겨집니다.
```ts
let x: number = 12; // Bad
let x = 12; // Good, 타입이 number로 이미 추론되어 있음

// Bad
const person: {
  name: string;
  born: {
    where: string;
    when: string;
  };
  // ...
} = {
  name: 'yhan',
  born: {
    where: 'Busan, South Korea',
    when: '25, 12, 1993',
  },
  // ...
};

// Good
const person = {
  name: 'yhan',
  born: {
    where: 'Busan, South Korea',
    when: '25, 12, 1993',
  },
  // ...
};
```

타입스크립트는 입력 받아 연산을 하는 함수가 어떤 타입을 반환하는지 정확히 알고 있습니다.
```ts
function square(nums: number[]) {
  return nums.map(x => x * x);
}
const squares = square([1, 2, 3, 4]); // 타입은 number[]
```
IDE가 추론가능한 타입에 대해서는 명시적 타입 구문이 필요하지 않습니다.

함수 및 메서드 시그니처에는 타입구문을 포함하고, 함수 내에서 생성된 지역변수에는 타입구문을 넣치 않는 것이 좋습니다. 타입구문을 생략하여 불필요한 코드들을 줄이고 로직에 집중할 수 있도록 하는 것이 좋습니다.

```ts
interface Product {
  id: string;
  name: string;
  price: number;
}

function logProduct(product: Product) {
  const id: number = product.id; //Error
  const name: string = product.name;
  const price: number = product.price;
  console.log(id, name, price);
}
```
logProduct 함수 내의 명시적 타입 구문이 없었더라면, 코드는 아무런 수정 없이도 타입 체커를 통과했을 겁니다.
```ts 
// Bad
function logProduct(product: Product) {
  const {id, name, price}: {id: string; name: string; price:number} = product;
  console.log(id, name, price);
}

// Good
function logProduct(product: Product) {
  const {id, name, price} = product;
  console.log(id, name, price);
}
```
비구조화 할당문은 모든 지역 변수의 타입이 추론되도록 합니다. 여기에 추가로 명시적 타입 구문을 넣는다면 불필요한 타입 선언으로 인해 코드가 번잡해집니다.

```ts
// Good
const elmo: Product = {
  name: 'Tickle Me Elmo',
  id: '048188 627152',
  price: 28.99,
};
```
객체 리터럴을 정의할 때는 타입을 명시해주면 오타 등의 오류를 잡는데 유용합니다 (잉여 속성 체크). 사용하는 곳이아닌, 선언하는 곳에서 오류를 발견할 수 있습니다.

```ts
interface Vector2D {
  x: number;
  y: number;
}
function add(a: Vector2D, b: Vector2D): Vector2D {
  return { x: a.x + b.x, y: a.y + b.y };
}
```
함수를 작성할 때도 반환타입을 구체적으로 명시함으로써 사용자가 타입명세를 확인할 때 혼동하지 않게 해주는 것이 좋습니다.

eslint의 no-inferrable-types 규칙을 사용하면 작성도니 타입구문이 정말로 필요한지 확인할 수 있습니다.