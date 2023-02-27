# string 타입보다 더 구체적인 타입 사용하기

변수의 범위를 보다 정확하게 표현하고 싶다면 string 타입보다는 문자열 리터럴 타입의 유니온을 사용하면 됩니다. 타입 체크를 더 엄격히 할 수 있고 생산성을 향상시킬 수 있습니다.

```ts
// Bad
interface Album {
  artist: string;
  title: string;
  releaseDate: string; // YYYY-MM-DD
  recordingType: string; // 예를 들어, "live" 또는 "studio"
}

const kindOfBlue: Album = {
  artist: "Miles Davids",
  title: "Kind of Blue",
  releaseDate: "August 17th 1959", // 날짜 형식이 다릅니다.
  recordingType: "Studio", // 오타 (대문자 S)
} // 에러가 안남

// Good
type RecordingType = 'studio' | 'live';

interface Album {
  artist: string;
  title: string;
  releaseDate: Date;
  recordingType: RecordingType;
}

const kindOfBlue: Album = {
  artist: "Miles Davids",
  title: "Kind of Blue",
  releaseDate: new Date("1959-08-17")
  recordingType: "Studio",
  // ~~~~~~~~~~~~'"Studio" 형식은 'RecordingType' 형식에 할당할 수 없습니다.
} // 에러 발생(정상)
```

이러한 방식의 세 가지 장점

1. 타입을 명시적으로 정의하므로써 다른 곳으로 값이 전달되어도 타입 정보다 유지됩니다.
2. 타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여 넣을 수 있습니다.
3. keyof 연산자로 더욱 세밀하게 객체의 속성 체크가 가능해집니다.

```ts
function pluck<T, K extends keyof T>(records: T[], key: K): T[K][] {
  return records.map((r) => r[key]);
}
```
