# 조건부 타입

## 제네릭에서 활용하기

```typescript
type StringNumberSwitch<T> = T extends number ? string : number;

let varA: StringNumberSwitch<number>; // string
let varB: StringNumberSwitch<string>; // number

function removeSpaces<T>(text: T): T extends string ? string : undefined; // 오버로드 시그니쳐
function removeSpaces<T>(text: any) { // 위에서 오버로딩을 해주었기에 any로 설정해주어도 된다
    if (typeof text === "string") { // 해당 조건문을 제대로 사용하기 위해 오버로딩 사용
        return text.replaceAll(" ", "");
    } else {
        return undefined;
    }
}

let result = removeSpaces("hi im winterlood");
result.toUpperCase();
let result2 = removeSpaces(undefined);

```

제네릭에서 조건부타입을 활용하면 타입을 더 엄격하게 정의할 수 있다.

## 분산

```typescript
type Exclude<T, U> = T extends U ? never : T;

type A = Exclude<number | string | boolean, string>;
// 1단계
// Exclude<number, string> |
// Exclude<string, string> |
// Exclude<boolean, string>

// 2단계
// number extends string ? never : number => number
// string extends string ? never : number => never
// boolean extends string ? never : number => boolean

// 결과
// number | never | boolean
```

그런데 결과에서 never는 생략된다.
왜냐하면 never는 공집합 타입이기 때문에 그냥 원본 집합일 뿐이다.
number와 never를 합집합 하면 결국 number라는 집합이 된다.

즉, 결과에 never가 포함되어 있으면 사라진다.

이는 T와 U가 동일한 타입일 경우는 타입을 제거하고, 다를 경우 T를 추출하는 방식이다.

```typescript
type Extract<T, U> = T extends U ? T : never;

type B = Extract<number | string | boolean, string>;
```

이렇게 특정 타입(string)만 추출하게끔 작업도 가능하다.

### 분산 방지

```typescript
type StringNumberSwitch<T> = [T] extends [number] ? string : number;

let a: StringNumberSwitch<boolean | number | string>; // number
```

이렇게 분산 방지를 하면 (`[T] extends [number]`)
유니온 타입을 전달해도 분산되지 않는다.

boolean, number, string의 합집합이 number가 아니니까 a는 number로 나온다.

## infer

> 함수 반환 값의 타입이 정해져있는 경우, 그 타입만 추출하고 싶을 때 사용
>
> inference 라는 의미의 infer이다.

```typescript
type FuncA = () => string;
type FuncB = () => number;

type ReturnType<T> = T extends () => infer R ? R : never;

type A = ReturnType<FuncA>;
type B = ReturnType<FuncB>;
type C = ReturnType<number>; // 타입 추론 불가로 never
```

만약 함수 `() => string;`인 FuncA 타입에서 string만 추출하고 싶다면
ReturnType 처럼 지정해주면 된다.

그러나 type C 처럼 number를 넣어주게 되면,

number extends () => infer R이 되는데
number의 수퍼타입이 존재하지 않으므로 추론이 불가하다.

그래서 조건문이 false로 최종 타입은 조건문에 따라 never가 된다.

any로 되는 것도 불가하다.

### Promise의 결과 타입만 떼어오기

```typescript
type PromiseUnpack<T> = T extends Promise<infer R> ? R : never;
// T는 프로미스 타입이어야 함
// 프로미스 타입의 결과값 타입을 반환해야 함

// number
type PromiseA = PromiseUnpack<Promise<number>>;
// string
type PromiseB = PromiseUnpack<Promise<string>>;
```

이렇게 infer를 통해 Promise 객체의 결과값 타입만을 반환할 수 있다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
