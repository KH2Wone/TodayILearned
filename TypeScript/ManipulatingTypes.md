# 타입 조작하기

## 인덱스드 액세스 타입

```typescript
type PostList = {
    title: string;
    content: string;
    author: {
        id: number;
        name: string;
        age: number;
    }
}[]

function printAuthorInfo(author: PostList[number]["author"]) {
    console.log(`${author.name}-${author.id}`)
}

const post: PostList[0] = {
    title: "게시글 제목",
    content: "게시글 본문",
    author: {
        id: 1,
        name: "강희원",
        age: 28,
    }
}

printAuthorInfo(post.author)
```

배열로 타입을 지정한 PostList를 하나의 요소만 있는 post에 타입을 지정하려면,
PostList[0] 혹은 PostList[number]로 선언하면 된다.

```typescript
const num = "number";
const author = "author";

PostList[num][author]
```

하지만 이렇게 변수를 통해 지정하는건 안된다.

"number", "author" 자체가 타입이기 때문이다.

## keyof 연산자

```typescript
interface Person {
    name: string;
    age: number;
}

function getPropertyKey(person: Person, key: keyof Person) {
    return person[key];
}

const person = {
    name: "강희원",
    age: 28
}

getPropertyKey(person, "name"); // 강희원
```

getPropertyKey 함수의 매개변수 key에 `"name" | "age"` 해줄 수도 있지만,
타입이 많아지거나 함수가 많아질 경우 수정할 때 수고로움이 발생한다.

이를 해결하기 위해 keyof를 사용하면 되는데, Person 타입의 key값을 가져와서
`"name" | "age"` 표현처럼 만들어준다.

```typescript
type Person = typeof person;

/*
* {
*   name: string;
*   age: number;
* }
* */

function getPropertyKey(person: Person, key: keyof typeof person) {
    return person[key];
}

const person = {
    name: "강희원",
    age: 28
}

getPropertyKey(person, "name"); // 강희원
```

typeof를 통해 타입을 추론해주는 것도 가능하다.

## 맵드 타입

```typescript
interface User {
    id: number;
    name: string;
    age: number;
}

type PartialUser = {
    [key in "id" | "name" | "age"]?: User[key];
}
type BooleanUser = {
    [key in keyof User]: boolean;
}
type ReadonlyUser = {
    readonly [key in keyof User]: boolean;
}

function fetchUser(): ReadonlyUser {
    // ... 기능
}

function updateUser(user: PartialUser) {
    // ... 수정하는 기능
}

updateUser({
    // id: 1,
    // name: "강희원",
    age: 22,
})
```

실무에서 타입스크립트를 사용하면서
수정된 값만 변경되도록 하고 싶을 때 타입을 지정하는 방법이다.

즉, id, name, age가 있지만 age만 변경했을 경우 fetchUser에 age 값만 보낼 수 있도록 한다.

interface에서는 사용할 수 없다.
타입별칭으로만 사용해야 한다.

## 템플릿 리터럴 타입

> string 리터럴 타입들을 기반으로 특정 패턴을 갖는 문자열 타입들을 만드는 기능

```typescript
type Color = "red" | "black" | "green";
type Animal = "dog" | "cat" | "chicken";

type ColoredAnimal = `${Color}-${Animal}`;
// "red-dog", "red-cat", "black-chicken"...
```

이렇게 타입별칭으로 템플릿 리터럴 타입을 만들 수 있다.

문자열을 여러가지로 표현할 수 있는 방법 중 하나이다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
