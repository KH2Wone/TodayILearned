# 선언 합침

> interface는 중복으로 선언하면 선언을 합치는 것으로 인식

```typescript
interface Person {
    name: string;
}

interface Person {
    age: string;
}
```

동일한 이름으로 interface를 선언 시, 합쳐지는 것으로 인식한다.

```typescript
interface Person {
    name: string;
}

interface Person {
    age: string;
    name: "hello"; // error
    // or
    name: number; // error
}
```

그러나 이렇게 서브타입인 string 리터럴 타입 혹은 아예 다른 타입인 number를 지정하는 것은 안된다.

동일한 프로퍼티를 동일한 타입으로 중복 선언은 가능하다.

```typescript
interface Developer extends Person {
    name: "hello";
}
```

그리고 `extends`로 확장한 경우에는 서브타입이기만 하면 지정이 가능하다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
