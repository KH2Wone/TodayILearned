# 제네릭

> 어디서든 사용할 수 있는 범용적인 함수

```typescript
function func<T>(value: T): T {
    return value;
}

let num = func(10);
let bool = func(true);
let str = func("string");
```

어떤 타입이 담길지 결정하는 것은 함수를 호출 할 때 결정이 되는 것이다.

## 튜플

```typescript
function func<T>(value: T): T {
    return value;
}

let arr = func<[number, number, number]>([1, 2, 3]);
```

이렇게 튜플 타입도 지정해줄 수 있다.

## 타입 변수 응용

### (1)

```typescript
function swap<T, U>(a: T, b: U) {
    return [b, a]
}

const [a, b] = swap("1", 2);
```

각각 다른 타입으로 지정하고 싶은 경우 `T`와 `U`로 지정해주면 된다.

### (2)

```typescript
function returnFirstValue<T>(data: [T, ...unknown[]]) {
    return data[0];
}

let num = returnFirstValue([0, 1, 2]);

let str = returnFirstValue([1, "hello", "mynameis"])
```

어차피 반환값이 data[0] 이기 때문에, 배열 내 첫번째(T)를 제외하고는 rest 파라미터로 unknown 즉,
나머지 요소들의 타입은 모르겠다 라는 의미이다.

이렇게 되면 data[0]에 number 타입이 들어오든, string 타입이 들어오든 커서를 올렸을 때
정확하게 일치하는 타입이 추론된다.

### (3)

```typescript
function getLength<T extends { length: number }>(data: T) {
    return data.length;
}

let var1 = getLength([1, 2, 3]);
let var2 = getLength("12345")
let var3 = getLength({length: 10});
let var4 = getLength(10); // error
```

var4 값처럼 length가 없는 타입은 매개변수로 받지 못하게 하려고 한다.

이 때 extends {length: number}를 해주면 number 타입의 length를 가지고 있는 객체를 확장하는 타입으로
T를 제한한다.

## map 메서드

```typescript
let arr = [1, 2, 3];

function map<T, U>(arr: T[], callback: (item: T) => U) {
    let result = [];
    for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i]))
    }
    return result;
}

map(arr, (it) => it * 2);
map(["hi", "hello"], (it) => parseInt(it));
```

두번째 함수호출 처럼 분명 배열 내 타입은 string이었는데 반환값은 number로 바뀔 수 있다.

이를 방지하기 위해 제네릭 타입에 U를 추가로 넣어주면 된다.

## forEach 메서드

```typescript
let arr2 = [4, 5, 6];

function forEach<T>(arr: T[], callback: (item: T) => void) {
    for (let i = 0; i < arr.length; i++) {
        callback(arr[i]);
    }
}

forEach(arr2, (it) => {
    console.log(it.toFixed());
})
```

다음과 같이 타입을 유연하게 적용하여 forEach를 만들 수 있다.

## 제네릭 인터페이스

```typescript
interface KeyPair<K, V> {
    key: K;
    value: V;
}

let keyPair: KeyPair<string, number> = {
    key: "key",
    value: 0,
}

let keyPair2: KeyPair<boolean, string[]> = {
    key: true,
    value: ["1"],
}
```

이처럼 인터페이스를 제네릭으로 만들어줄 수 있다.

## 인덱스 시그니처

```typescript
interface Map<V> {
    [key: string]: V;
}

let stringMap: Map<string> = {
    key: "value"
}
```

## 타입 별칭

```typescript
type Map2<V> = {
    [key: string]: V;
}
```

## Promise

```typescript
const promise = new Promise<number>((resolve, reject) => {
    setTimeout(() => {
        resolve(20);
        reject("~때문에 실패")
    }, 3000);
})

promise.then(response => {
    // response: unknown
    console.log(response * 10);
})

promise.catch(err => {
    // err: any
    if (typeof err === "string") {
        console.log(err);
    }
})
```

promise는 제네릭 클래스를 기반으로 선언되어 있기 때문에
타입 변수로 결과값의 타입을 정의해줄 수 있지만 실패했을 때의 타입은 지정할 수 없다.

### 반환값

```typescript
interface Post {
    id: number;
    title: string;
    content: string;
}

function fetchPost(): Promise<Post> {
    return new Promise((resolve, reject) => {
        // new Promise<Post>도 가능
        setTimeout(() => {
            resolve({
                id: 1,
                title: "게시글 제목",
                content: "게시물 컨텐츠"
            })
        }, 3000);
    })
}
```

주석 내용처럼 new Promise<Post>로 타입을 정해줘도 좋지만, 협업 할 때에
fetchPost 함수 앞에 Promise<Post>라고 명시해주면 복잡한 코드 일수록 알아보기 쉽다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
