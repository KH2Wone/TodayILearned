# 객체 타입의 호환성

> TypeScript는 구조적 타입 시스템이다.

```typescript
type Animal = { // super 타입
    name: string;
    color: string;
}
type Dog = { // sub 타입, Animal에 포함될 수 있음
    name: string;
    color: string;
    breed: string;
}

let animal: Animal = {
    name: "기린",
    color: "yellow"
}
let dog: Dog = {
    name: "돌돌이",
    color: "brown",
    breed: "진도"
}

animal = dog; // 업 캐스팅
dog = animal; // error! - 다운 캐스팅
```

여기서 animal 안에 dog가 포함되기 때문에 업 캐스팅 할 수 있지만, dog에 animal을 다운 캐스팅이 되어 넣을 수 없다.

## 초과 프로퍼티 검사

```typescript
type Book = {
    name: string;
    price: number;
}
type ProgrammingBook = {
    name: string;
    price: number;
    skill: string;
}

let book: Book;
let programmingBook: ProgrammingBook = {
    name: "한 입 크기로 잘라먹는 타입스크립트",
    price: 33000,
    skill: "typescript"
};
book = programmingBook; // pass
```

위에 살펴본 것과 같이 마지막 코드는 에러가 발생하지 않는다.
여기서 더 나아가,

### 객체

```typescript
let book2: Book = {
    name: "한 입 크기로 잘라먹는 타입스크립트",
    price: 33000,
    skill: "typescript" // error!
}
let book3: Book = programmingBook; // pass
```

Book 타입의 객체 book2에 skill이라는 프로퍼티를 하나 더 넣는다고 하면 에러가 발생한다.

그러나 book3는 에러가 발생하지 않고 정상 작동 된다.

이는 객체 리터럴을 사용한게 아니라 변수에 담아 넣어주었기 때문에 그렇다.

### 함수

```typescript
function func(book: Book) {};
func({
    name: "한 입 크기로 잘라먹는 타입스크립트",
    price: 33000,
    skill: "typescript" // error!
});
func(programmingBook); // pass
```

함수의 매개변수에 Book 타입을 넣은 후 객체 리터럴을 넘겨준다면 이 또한 에러가 발생한다.

그러나 `func(programmingBook);` 이렇게 변수에 담아 넘겨주게 되면 에러를 피할 수 있다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
