# 함수 오버로딩

> 하나의 함수를 매개변수의 개수나 타입에 따라
> 여러가지 버전으로 만드는 문법

## 오버로드 시그니쳐

```typescript
// 오버로드 시그니쳐
function func(a: number): void; // - 1버전
function func(a: number, b: number, c: number): void; // - 2버전

function func() {
} // 구현 시그니처

func(); // error
func(1); // - 1버전
func(1, 2); // error
func(1, 2, 3); // - 2버전
```

이렇게 에러가 발생하는 이유는 오버로드 시그니쳐를 만들어두었기 때문이다.

실제 구현부에 있는 매개변수를 따라가는 것이 아니라 오버로드 시그니쳐를 따라간다.

## 여러 타입 버전의 함수 만들기

```typescript
function func(a: number): void; // - 1버전
function func(a: number, b: number, c: number): void; // - 2버전

function func(a: number, b?: number, c?: number) {
    // 매개변수 b, c가 들어가있으면 1버전이 필요없어져서 에러가 발생한다.
    // 방지하기 위해 옵셔널 체이닝을 넣어준다.

    if (typeof b === "number" && typeof c === "number") {
        console.log(a + b + c);
    } else {
        console.log(a * 20);
    }
}

func(1);
func(1, 2, 3);
```

이렇게 매개변수가 1개일 때의 버전, 3개일 때의 버전을 만들 수 있다.

## interface에서 오버로딩

```typescript
interface Person {
    readonly name: string;
    age?: number;

    sayHi(): void;

    sayHi(a: number, b: number): void;
}

const person: Person = {
    name: "강희원",
    sayHi: function () {
        console.log("Hi");
    }
}

person.sayHi();
person.sayHi(1, 2);
```

`sayHi(): void;` 이렇게 함수 호출 시그니쳐로 표현해줘야 한다.

```typescript
interface Person {
    sayHi: () => void;
    sayHi: (a: number, b: number) => void; // error
}
```

이렇게 함수타입 표현식으로 표현하면 중복되었다는 에러가 발생한다.

오버로딩을 구현하고 싶다면 호출 시그니쳐를 사용해야 한다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
