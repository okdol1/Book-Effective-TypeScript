# 사용할 때는 너그럽게, 생성할 때는 엄격하게

매개변수 타입의 범위는 넓게 설계해야 좋고, 반환 타입의 범위는 좁게 설계해야 좋다.

다음 예제를 보면 LngLatBounds 는 총 19가지 (9 + 9 + 1)형태를 지원하고 있다. 이렇게 19가지 경우의 수를 지원하는 것은 (심지어 다양한 타입을 지원해야하는 라이브러리 설계에서도) 좋은 설계가 아니다. 또, 한 함수의 반환 타입(CameraOptions)이 다른 함수의 매개변수 타입으로 사용되고 있다. 이렇게 되면 반환타입이 느슨해지거나, 매개변수 타입이 엄격해지는 문제가 생긴다.

```ts
declare function setCamera(camera: CameraOptions): void;
declare function vewportForBounds(bounds: LngLatBounds): CameraOptions;

type LngLat =
  | { lng: number; lat: number }
  | { lon: number; lat: number }
  | [number, number];

type LngLatBounds =
  | { northeast: LngLat; southwest: LngLat }
  | [LngLat, LngLat]
  | [number, number, number, number];

interface CameraOptions {
  center?: LngLat;
  zoom?: number;
  bearing?: number;
  pitch?: number;
}
```

위 예제에서는 반환값인 cameraOptions의 형태가 너무 자유롭기 때문에, 반환받은 값을 사용하다보면 다음과 같은 불편함이 생긴다.

```ts
const cameraOptions = viewportForBounds(bounds);
const { center: {lat, lan}, zoom} = cameraOptions;
// 에러,          ~~~      ... 형식에 'lat' 속성이 없습니다.
//                    ~~~ ... 형식에 'lan' 속성이 없습니다.


cameraOptions를 사용하기 편리한 반환값으로 개선하려면, 유니온 타입에 각각을 코드 상에서 분기 처리하는 것이 좋다. 분기 처리를 위한 방법 중에 하나로 Sth, SthLike 이렇게 유사 타입을 만들어볼 수 있다.

declare function setCamera(camera: CameraOptions): void; // 느슨한 매개변수 타입
declare function viewportForBounds(bounds: LngLatBounds): Camera; // 엄격한 반환값 타입

interface LngLat { lng: number; lat: number; };

type LngLatLike =
  LngLat |
  { lon: number; lat: number; } |
  [number, number]

interface Camera { // 엄격한 반환값 타입
  center: LngLat;
  zoom: number;
  bearing;: number;
  pitch: number;
}

interface CameraOptions { // 느슨한 매개변수 타입
  center?: LngLatLike;
  zoom?: number;
  bearing;: number;
  pitch?: number;
}
```
