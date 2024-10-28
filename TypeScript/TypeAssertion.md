# 타입 단언

> 초기화 상태의 값을 단언

```typescript
type Person = {
    name: string;
    age: number;
}

let person: Person = {};
person.name = "강희원"; // error
person.age = 28; // error
// or
let person2: any = {}; // nope!!!
```

빈 객체에 값을 넣는다고 했을 때 타입을 지정하면 에러가 발생한다.

그렇다고 any를 넣을 수도 없고, 빈 객체 상태일 때는 타입을 어떻게 지정하는 것이 좋을까?

```typescript
let person = {} as Person;
person.name = "강희원";
person.age = 28;
```

이처럼 타입 단언을 해주면 초기화된 person 값에 Person 타입이 들어오게 할 수 있다.

그리고 추가 프로퍼티 검사로 에러가 발생할 때 타입 단언으로 해결할 수 있다.

```typescript
type Dog = {
    name: string;
    color: string;
}

let dog: Dog = {
    name: "돌돌이",
    color: "brown",
    breed: "진도" // error
}
```

```typescript
let dog = {
    name: "돌돌이",
    color: "brown",
    breed: "진도"
} as Dog; // pass
```

## 규칙

아무 때나 쓰는건 안된다.

**값 as 단언**을 **A as B**로 예를 들어

A가 B의 슈퍼타입이거나,
A가 B의 서브타입이어야 한다.

```typescript
let num1 = 10 as never; // pass
let num2 = 10 as unknown; // pass

let num3 = 10 as string; // error
```

number와 string 타입은 교집합이 없다.

슈퍼타입이거나 서브타입이 아니기 때문에 num3는 에러가 발생한다.

그럼에도 해결할 수 있는 방법이 있는데

```typescript
let num4 = 10 as unknown as string; // pass
```

다중 단언을 하면 에러가 발생하지 않지만, 좋은 방법은 아니다.

## const 단언

> const로 선언한 것과 동일한 효과를 볼 수 있다.

```typescript
let num5 = 10 as const;
```

num5는 `let num5 = 10` 일 때 number로 타입이 추론되지만, as const를 붙여주게 되면 
number 리터럴 타입 10으로 추론된다.

### 객체에서의 활용

> readonly를 한번에

```typescript
let cat = {
    name: "야옹이",
    color: "yellow"
} as const;

cat.name = '' // error
```

원래 기존에 name과 color를 string으로 타입 추론이 되었다면,

객체에 as const를 추가하면 모든 프로퍼티가 읽기 전용(readonly)으로 바뀌는 것을 볼 수 있다.

타입 또한 `name: "야옹이", ...` string 리터럴 타입으로 지정된다.

이렇게 되면 객체 하나하나에 readonly를 붙여주지 않아도 된다.

## Non Null 단언

> 어떤 값이 null이거나 undefined가 아니라고 알려주는 단언

```typescript
type Post = {
    title: string;
    author?: string;
}

let post: Post = {
    title: "게시물1",
    author: "강희원"
}

const len: number = post.author?.length; // error
```

이 때 len에서 에러가 발생하는데, 이유는 author 뒤에 옵셔널 체이닝을 사용하여 해당 값이 undefined이 될 수 있기 때문이다.

이럴 때는 물음표를 느낌표로 바꿔주면 에러가 사라진다.

```typescript
const len: number = post.author!.length; // pass
```

무조건 있어! (ㅋㅋㅋ) 라는 의미로 생각해도 좋다.

여기까지 설명한 타입 단언은 말 그대로 있다고 단언하는 수준이기 때문에 타입스크립트의 눈을 가리는 용도일 뿐이다.

이를 함부로 남용해서는 안되겠다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
