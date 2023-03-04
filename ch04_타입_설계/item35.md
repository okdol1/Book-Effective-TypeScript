# 데이터가 아닌, API와 명세를 보고 타입 만들기

데이터에 드러나지 않는 예외적인 경우들이 문제가 될 수 있기 때문에 데이터보다는 명세로부터 코드를 생성하는 것이 좋습니다.

```
$npm i --save-dev @types/geojson
```

```ts
import { Feature } from "geojson";

function calculateBox(f: Feature): BoundingBox | null {
  let box: BoundingBox | null = null;
  const helper = (coords: any[]) => {
    // ...
  };
  const { geometry } = f;
  if (geometry) helper(geometry.coordinates); // ❌ 'Geometry'형식에 'coordinates'속성이 없습니다.
}
```

geometry에 coordinates 속성이 있다고 가정한 것이 문제, GeoJSON은 GeometryCollection(coordinates 속성이 없음)일 수 있다.

```ts
// Not Bad
const { geometry } = f;
if (geometry) {
  if (geometry.type === "GeometryCollection") {
    throw new Error("GeometryCollections are not supported");
  }
  helper(geometry.coordinates); // 정상
}
```

GeometryCollection 타입을 차단하기보다는 모든 타입을 지원하는 것이 더 좋은 방법이기 때문에 조건을 분기해서 헬퍼 함수를 호출하면 모든 타입을 지원할 수 있습니다.

```ts
// Good
const gometryHelper = (g: Geometry) => {
  if (geometry.type === "GeometryCollection") {
    throw new Error("GeometryCollections are not supported");
  } else {
    helper(geometry.coordinates); // 정상
  }
};

const { geometry } = f;
if (geometry) {
  gometryHelper(geometry);
}
```
