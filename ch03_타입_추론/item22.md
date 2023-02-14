# 타입 좁히기

> 타입 좁히기는 타입스크립트가 넓은 타입으로부터 좁은 타입으로 진행하는 과정을 말한다.

### null 체크

```ts
const el = document.getElementById("foo"); // HTMLElement | null
if (el) {
  el; // HTMLElement
  el.innerHTML = "Party Time".blink();
} else {
  el; // null
  alert("No element #foo");
}
```

### instanceof

```ts
function contains(text: string, search: string | RegExp) {
  if (search instanceof RegExp) {
    search; // RegExp
    return !!search.exec(text);
  }
  search; // string
  return text.includes(search);
}
```

### typeof

- 자바스크립트에서 typeof null 이 "object"이기 문에, if 구문에서 null이 제외되지 않음

```ts
const el = document.getElementById("foo"); // HTMLElement | null
if (typeof el === "object") {
  el; // HTMLElement | null
}
```

### in

```ts
interface A {
  a: number;
}
interface B {
  b: number;
}
function pickAB(ab: A | B) {
  if ("a" in ab) {
    ab; // A
  } else {
    ab; // B
  }
}
```

### 내장함수

```ts
function contains(text: string, terms: string | string[]) {
  const termList = Array.isArray(terms) ? terms : [terms];
  termList; // string[]
}
```

### 태그된 유니온

```ts
interface UploadEvent {
  type: "upload";
  filename: string;
  contents: string;
}
interface DownloadEvent {
  type: "download";
  filename: string;
}
type AppEvent = UploadEvent | DownloadEvent;

function handleEvent(e: AppEvent) {
  switch (e.type) {
    case "download":
      e; // DownloadEvent
      break;
    case "upload":
      e; // UploadEvent
      break;
  }
}
```

### 커스텀 타입 가드

```ts
function isInputElement(el: HTMLElement): el is HTMLInputElement {
  return "value" in el;
}

function getElementContent(el: HTMLElement) {
  if (isInputElement(el)) {
    el; // HTMLInputElement
    return el.value;
  }
  el; // HTMLElement
  return el.textContent;
}
```
