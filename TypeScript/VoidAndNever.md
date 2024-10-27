# void와 never 타입

## void

> 아무것도 없음을 의미하는 타입

```typescript
function func1(): void {
    console.log("Hello");
}
```

실제로 return 하는 값이 아무것도 없기 때문에 void 타입으로 지정하는 것이 맞다.

또한, void 타입일 경우 다른 타입의 값은 절대 들어갈 수 없으며 오직 `undefined`만 들어갈 수 있다.

tsconfig.json에 `compilerOptions > strictNullChecks`를 false로 하면 `null`도 넣을 수 있다.

### 타입을 `undefined`나 `null`로 지정하면?

아무것도 없는 값이고 `undefined` 값이 들어갈 수 있는 건데 실제 타입을 `undefined`나 `null`로 지정하면 에러가 발생한다.

그렇다면 이 두 가지 타입을 지정하려면 어떻게 바꿔져야 할까?

```typescript
// undefined (1)
function func2: undefined {
    console.log("Hello");
    return; // undefined를 반환하는 것과 동일함
}

// undefined (2)
function func3: undefined {
    console.log("Hello");
    return undefined;
}

// null
function func4: null {
    console.log("Hello");
    return null; // undefined (1) 처럼 return만 하는건 안됨
}
```

이 경우만 가능하다.

즉, return 문을 아예 사용하고 싶지 않을 때 `void`를 사용하면 된다.

## never

> 불가능(모순)을 의미하는 타입

```typescript
function func5: never {
    while(true) {}
}
```

이처럼 정상적으로 종료될 수 없어서 해당 함수에 반환값이 있는게 모순이라는 의미일 때 `never` 타입을 사용한다.

```typescript
function func6: never {
    throw new Error();
}
```

이것 또한 실행되면 프로그램이 바로 중지될 것이기에 `never` 타입이 적합하다.

### 다른 타입의 값을 넣는다면?

`void`와 달리 `undefined`, `null`(strictNullChecks 모드 false) 값을 대입하는 것이 불가하다.

심지어 `any` 타입의 값도 불가하다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
