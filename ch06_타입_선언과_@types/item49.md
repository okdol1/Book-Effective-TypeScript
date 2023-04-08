# 콜백에서 this에 대한 타입 제공하기

- this 바인딩이 동작하는 원리를 이해해야 한다.
- 콜백 함수에서 this를 사용해야 한다면, 타입 정보를 명시해야 한다. this는 동적 스코프(호출된 방식에 따라 값이 달라진다)라 예상하기 어렵기 때문이다.

```ts
//콜백 함수 첫 번째 매개변수에 있는 this는 특별하게 처리 된다.
//-> 실제로 인자로 넣을 필요는 없다. this 바인딩 체크용이다.
//콜백 함수의 매개변수에 this를 추가하면 this 바인딩을 체크할 수 있다.
function addKeyListener(
  el: HTMLElement,
  fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
  el.addEventListener("keydown", (e) => {
    fn(el, e); //❌
    //1개의 인수가 필요한데 2개를 가져왔습니다.
  });
}

function addKeyListener2(
  el: HTMLElement,
  fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
  el.addEventListener("keydown", (e) => {
    fn(e); //this 바인딩 체크해준다.
    //'void' 형식의 'this' 컨텍스트를 메서드의 'HTMLElement' 형식 'this'에 할당할 수 없습니다
  });
}

//콜백 함수를 call로 호출해서 해결할 수 있다.
function addKeyListener3(
  el: HTMLElement,
  fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
  el.addEventListener("keydown", (e) => fn.call(el, e));
}
```
