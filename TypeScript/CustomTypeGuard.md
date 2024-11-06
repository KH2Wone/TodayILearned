# 사용자 정의 타입 가드

> 사용자가 타입 가드를 정의할 수 있다.

```typescript
type Dog = {
    name: string;
    isBark: boolean;
}
type Cat = {
    name: string;
    isScratch: boolean;
}
type Animal = Dog | Cat;

function warning(animal: Animal) {
    if("isBark" in animal) {
        // 강아지
        animal;
    } else if ("isScratch" in animal) {
        // 고양이
    }
}
```

원래 이렇게 타입 좁히기를 해줬었는데, 이렇게 되면 수정할 수 없는 타입이거나 이후에 key값이 바뀔 수 있다면
타입 좁히기를 정상적으로 시행할 수 없다.

## 사용자 정의 타입 가드 만들기

```typescript
function isDog(animal: Animal) {
    // as Dog를 해주어야 Dog로 타입이 단언되어 타입이 좁혀진 채로 isBark를 판단 가능
    return (animal as Dog).isBark !== undefined;
}
function isCat(animal: Animal) {
    return (animal as Cat).isScratch !== undefined;
}
```

그런데 제대로 타입 좁히기가 되지 않는다.

왜냐하면 우리가 직접 만든 함수의 반환값으로는 타입을 잘 좁혀주지 않는다.

그래서 함수 자체를 타입가드 역할을 하도록 만들어줘야 한다.

```typescript
function isDog(animal: Animal):animal is Dog {
    return (animal as Dog).isBark !== undefined;
}
function isCat(animal: Animal):animal is Cat {
    return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
    if(isDog(animal)) {
        // 강아지
        animal;
    } else if (isCat(animal)) {
        // 고양이
        animal;
    }
}
```

만약 함수가 true를 리턴한다면 animal은 Dog 타입이다 라는 의미이다.

이렇게 하면 제대로 타입이 좁혀진다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
