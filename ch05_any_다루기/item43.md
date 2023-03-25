# 몽키 패치보다는 안전한 타입을 사용하기

몽키패치(Monkey Patch)는 런타임 동안에 동적으로 객체에 메서드나 프로퍼티를 추가하는 패턴을 일컫는다. 원래는 예고없이 깜짝스럽게 진행된다는 뜻의 게릴라 패치(Guerrilla Patch)가 비슷한 발음의 고릴라 패치(Gorilla Patch)라고 불리던 중, 비개발자인 CEO에게 좀 위협적으로 들리기 때문에 고릴라 보다는 귀여운 원숭이로 바꿔 부르게 되었다는 유래가 있다.

몽키패치는 안티패턴으로 지양해야하지만 어쩔 수 없이 사용해야하는 경우가 발생할 수 있다. 그런데 타입스크립트에서 몽키패치는 타입체커가 임의로 추가한 속성에 대한 정보를 갖고있지 않다는 점에서 문제가 생긴다.

```ts
document.monkey = "Magic";
// Document 타입에 'monkey' 프로퍼티가 없습니다.
```

이는 interface의 보강(augmentation) 기능을 사용해서 해결할 수 있다.

```ts
interface Document {
  monkey: string; // 기존의 Document 인터페이스에 monkey 속성이 추가됨.
}
```

보강과 같이 전역적으로 적용되는 것을 피하고 싶다면 extends 키워드로 확장해서 사용하는 방법이 있다.

```ts
interface MonkeyDocument extends Document {
  monkey: string;
}

(document as MonkeyDocument).monkey = "Magic";
```

그러나 몽키 패치를 남용해서는 안 되며, 더 궁극적으로 설계가 잘된 구조로 리팩터링하는 것이 좋다.
