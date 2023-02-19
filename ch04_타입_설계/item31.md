# 타입 주변에 null 값 배치하기

null 값은 어디에 어떻게 배치하면 좋을까? 타입스크립트가 null 값 사이의 관계를 컨텍스트를 통해 잘 이해할 수 있도록 돕는 관점에서 배치해야 한다.

A와 B의 null 여부가 서로에게 영향이 있는 경우를 생각해보자. 예를 들어 A가 null 일 때 B도 반드시 null이고, A가 null이 아닐 때 B도 반드시 null이 아니라면, 각각을 독립적인 타입으로 두어 ' | null ' 을 추가해주는 것은 좋은 방법이 아니다. A와 B를 묶어 큰 객체로 타입을 만들고 그 전체가 null 이거나 null이 아니게 만들어야 한다. 이렇게 만들면 각 타입을 다루기 훨씬 쉬워진다.

아래의 예제에서 min, max는 각각 number | undefined 이고 반환값도 (number | undefined)[] 이다. 객체에 undefined가 포함되는 것은 지양해야 한다.

```ts
// 개선 전
function extent1(nums: number[]) {
  let min, max;

  for (const num of nums) {
    if (!min) { // falsy 값일 때 문제 O
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
                   // ~~~ 'number | undefined' 타입을 number 타입에 할당할 수 없습니다.
    }
  }
}

// 개선 후
function extent2(nums: number[]) {
  let result: [number, number] | null = null; // 연관있는 값을 묶어 null 배치

  for( const num of nums) {
    if (!result) {
      result = [num, num];
    } else {
      result = [ Math.min(num, result[0]), Math.max(num, result[1]) ];
    }
  return result;
}
```
