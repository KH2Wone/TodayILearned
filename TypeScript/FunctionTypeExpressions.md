# 함수 타입 표현식과 호출 시그니쳐

> 함수의 타입을 별도로 정의

## 함수 타입 표현식

```typescript
type Operation = (a: number, b: number) => number;

const add: (a: number, b: number) => number = (a, b) => a + b;
const sub: Operation = (a, b) => a - b;
```

type으로 지정하여 한번에 Operation을 지정할 수 있다.

## 호출 시그니쳐

```typescript
type Operation2 = {
    (a: number, b: number): number; // 이 부분만 떼어서 타입 지정이 가능
    name: string; // 이렇게 추가도 가능
}

const add2: Operation2 = (a, b) => a + b;
const sub2: Operation2 = (a, b) => a - b;
```

함수 타입 표현식처럼 사용할 수 있다.

객체처럼 타입을 넣는 이유는 자바스크립트의 함수가 객체 타입이기 때문이다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
