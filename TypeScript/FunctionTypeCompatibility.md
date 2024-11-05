# 함수 타입의 호환성

> 특정 함수 타입을 다른 함수 타입으로 취급해도 괜찮을까?

## 반환값 호환

```typescript
type A = () => number;
type B = () => 10;

let a: A = () => 10;
let b: B = () => 10;

a = b;
b = a; // error!
```

a = b는 되지만 b = a는 안되는 이유는, a <- b로 가는 것은 업캐스팅이니까 가능하다.

하지만  b <- a는 다운캐스팅이라 불가하다.

## 매개변수 호환

```typescript
type C = (value: number) => void;
type D = (value: 10) => void;

let c: C = (value) => {};
let d: D = (value) => {};

c = d; // error!
d = c;
```

반환값 호환과 달리 매개변수는 반대로 작동한다.

분명 c <- d는 업캐스팅이라 가능해야되는데 왜 안될까?

매개변수는 뜻하지 않은 에러가 발생하지 않게 하기 위해 이렇게 작동한다.

```typescript
type Animal = {
    name: string;
}

type Dog = {
    name: string;
    color: string;
}

let animalFunc = (animal: Animal) => {
    console.log(animal.name);
}

let dogFunc = (dog: Dog) => {
    console.log(dog.name);
    console.log(dog.color);
}

animalFunc = dogFunc; // error!
dogFunc = animalFunc;
```

이 또한 반환값 호환과 반대로 작동한다.

왜냐하면,

```typescript
let testFunc = (animal: Animal) => {
    console.log(animal.name);
    console.log(animal.color); // error! - 당연함. color 타입이 없음.
}
```

이렇게 할당을 허용해줘버리면 말도안되는 코드가 나올 수 있기 때문에 업캐스팅을 막는 것이다.

## 매개변수 개수 차이 호환

```typescript
type Func1 = (a: number, b: number) => void;
type Func2 = (a: number) => void;

let func1 = Func1 = (a, b) => {};
let func2: Func2 = (a) => {};

func1 = func2;
func2 = func1; // error!
```

2개 <- 1개는 가능하지만, 1개 <- 2개는 호환될 수 없다.

할당하려는 타입의 매개변수보다 더 적은 개수의 매개변수를 가졌을 때만 호환될 수 있다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
