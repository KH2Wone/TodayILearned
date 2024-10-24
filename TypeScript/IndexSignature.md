# Index Signature

> 객체 타입의 정의를 더 유연하게 할 수 있도록 돕는다.

## 예시

```typescript
type CountryCodes = {
    Korea: string;
    UnitedState: string;
    UnitedKingdom: string;
    // ...
}

let countryCodes: CountryCodes = {
    Korea: "ko",
    UnitedState: "us",
    UnitedKingdom: "uk",
    // ...
}
```

객체 안의 요소가 수 없이 많아진다면 타입을 하나하나 넣어주다가 하루가 끝난다..!

이를 한번에 해결할 수 있는 방법은? -> 규칙을 보자

`String(key): String(value)` 형태로 구성되어 있는 것을 확인할 수 있다.

그렇다면 이를 한번에 타입 지정해주자.

## 해결법

```typescript
type CountryCodes = {
    [key: string]: string; // Index Signature
}

let countryCodes: CountryCodes = {
    Korea: "ko",
    UnitedState: "us",
    UnitedKingdom: "uk",
    // ...
}
```

이를 인덱스 시그니처라고 한다.

## 심화

```typescript
type CountryNumberCodes = {
    [key: string]: number;
}

let countryNumberCodes: CountryNumberCodes = {
    Korea: 410,
    UnitedState: 840,
    UnitedKingdom: 826
}
```

문제 없는 코드다. 하지만 만약,

```typescript
let countryNumberCodes: CountryNumberCodes = {};
```

이렇게 하면 에러가 발생할까? -> 답은 **"발생하지 않는다."** 이다.

`[key: string]: number;` 해당 인덱스 시그니처 규칙을 위반하지 않는다면 모든 객체를 허용한다.

(아무런 요소가 없으니 위반할 규칙도 존재하지 않음)

### 반드시 존재해야만 하는 요소가 있을 때?

```typescript
type CountryNumberCodes = {
    [key: string]: number; // 이 규칙을 위반하지 않는 이상 에러가 나지 않음
    Korea: number;
}

let countryNumberCodes: CountryNumberCodes = {}; // error!
```

이렇게 되면 에러가 발생한다. Korea라는 요소가 반드시 있어야한다고 정의했기 때문이다.

```typescript
type CountryNumberCodes = {
    [key: string]: number; // 이 규칙을 위반하지 않는 이상 에러가 나지 않음
    Korea: number; // 반드시 Korea라는 값이 있어야할때
}

let countryNumberCodes: CountryNumberCodes = {
    Korea: 410
}
```

이렇게 작성하면 올바른 코드이다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
