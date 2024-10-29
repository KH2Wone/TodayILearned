# 타입 좁히기

> 조건문 등을 통해 넓은 타입에서 좁은 타입으로 타입을 상황에 따라 좁히기

## typeof, instanceof, in으로 타입을 좁히기

```typescript
type Person = {
    name: string;
    age: number;
};

function func(value: number | string | Date | null | Person) {
    if (typeof value === "number") {
        console.log(value.toFixed());
    } else if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else if (value instanceof Date) {
        console.log(value.getTime());
    } else if (value && "age" in value) {
        console.log(`${value.name}은 ${value.age}살 입니다.`)
    }
};
```

이를 타입 가드라고 한다.

다음과 같이 유니온 타입이 들어왔을 때 조건으로 타입을 좁힐 수 있다.

타입 좁히기를 하지 않고, `value.toFixed()` 등을 사용하게 되면 에러가 발생한다.

그 중 Date, in에 대해 자세히 알아보자.

### Date

```typescript
else if (value instanceof Date) {
        console.log(value.getTime());
}
```

이 부분을 살펴보면 처음에 `typeof value === "object"`를 작성할 경우 `null`이 유니온 타입에 포함될 때 에러가 발생한다.

왜냐하면 `null` 또한 object로 인식하기 때문이다.

이를 방지하기 위해 `instanceof`를 사용한다. 정확한 타입 좁히기를 하기 위함이다.

Date는 자바스크립트 인스턴스에 포함되어 있기 때문에 사용할 수 있다.

`instanceof`는 객체가 특정 클래스에 속하는지 확인할 수 있는데, value가 Date 객체에 포함되는지 확인한다.

### in

만약 정의한 Person이라는 타입도 `instanceof`로 타입 좁히기를 해준다면 에러가 발생한다.

왜냐하면 Person은 말그대로 타입이라서 그렇다.

`instanceof`는 왼쪽에 오는 값이 오른쪽에 있는 클래스의 인스턴스인지 확인하는 연산자이기 때문이다.

이를 해결하기 위해 객체 안에 특정 요소가 있는지 확인하기 위해 `in`을 사용한다.

```typescript
else if (value && "age" in value) {
    console.log(`${value.name}은 ${value.age}살 입니다.`)
}
```

그런데 `"age" in value`만 사용하면 에러가 발생하는데, 이는 value라는 값이 `null`일 수 있기 때문이다.

in 연산자 뒤에는 `null` 혹은 `undefined`가 들어오면 안된다.

이를 방지하기 위해 value라는 값이 있다고 가정 할 때 그 안에 age라는 요소가 정확히 존재하는지 확인하는 조건문을 작성해주면 된다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
