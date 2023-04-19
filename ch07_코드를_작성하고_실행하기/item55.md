# DOM 계층 구조 이해하기

타입스크립트에서는 DOM 엘리먼트의 계층 구조를 파악하기 유용하다.

Element와 EventTarget에 달려 있는 Node의 구체적인 타입을 안다면 타입 오류를 디버깅할 수 있고, 언제 타입 단언을 사용해야 할 지 알 수 있다.

### 계층 구조에 따른 타입의 예시

| 타입              | 예시                         |
| ----------------- | ---------------------------- |
| EventTarget       | window, XMLHttpRequest       |
| Node              | document, Text, Comment      |
| Element           | HTMLElement, SVGElement 포함 |
| HTMLElement       | `<i>`, `<b>`                 |
| HtmlButtonElement | `<button>`                   |

HTML 태그 값에 해당하는 'button' 같은 리터럴 값을 사용하여 DOM에 대한 정확한 타입을 얻을 수 있다.

```ts
document.getElementsByTagName("p")[0]; // HTMLParagraphElement
document.createElement("p"); // HTMLButtonElement
document.querySelector("div"); // HTMLDivElement
```

그러나 항상 정확한 타입을 얻을 수 있는 것은 아니다. 특히 document.getElementById에서 문제가 발생하게 된다.

```ts
document.getElementsByTagName("my-div"); // HTMLElement
```

일반적으로 단언문을 지양해야하지만 타입스크립트보다 우리가 더 정확히 알고 있는 경우이므로 단언문을 사용해도 좋다.

```ts
document.getElementsByTagName("my-div") as HTMLDivElement;
```

### Event 타입 별도의 계층 구조

- UIEvent: 모든 종류의 사용자 인터페이스 이벤트
- MouseEvent: 클릭처럼 마우스로부터 발생되는 이벤트
- TouchEvent: 모바일 기기의 터치 이벤트
- WheelEvent: 스크롤 휠을 돌려서 발생되는 이벤트
- KeyboardEvent: 키 누름 이벤트