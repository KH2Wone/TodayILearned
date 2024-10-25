# Any와 Unknown 타입

## Any

> 어떤 타입이든 담을 수 있게 해준다.

```typescript
let anyVar: any = 10;
anyVar = "Hello!";
anyVar = true; 
```

타입스크립트의 타입검사를 패스해준다.

```typescript
let anyVar: any = 10;
let num: number = 10;
num = anyVar;
```

이처럼 any 타입의 값(anyVar)을 number 타입의 변수(num)에 담을 수도 있다.

그냥 써도 될까? 싶겠지만 ts-node를 실행해보면,

```typescript
let anyVar: any = 10;
anyVar = () => {};
anyVar.toUpperCase(); // error!
```

에러가 발생한다. 당연하게도 anyVar는 함수인데 toUpperCase라는 메서드를 사용하니 에러가 발생하는 것이다.

이렇게 되면 타입검사를 안하는거나 다름이 없으며 런타임에 에러가 발생되기 때문에 타입스크립트를 사용하는 의미가 없어진다.

## Unknown

> Unknown도 Any처럼 어떤 타입이 들어와도 담을 수 있게 해준다.
> 
> 단, 차이점이 있는데 any처럼 특정 타입의 변수에 unknown 타입의 값을 넣을 수 없다.

```typescript
let unknownVar: unknown;

unknownVar = "";
unknownVar = 0;
unknownVar = () => {};
// ㄴ 여기까지 모두 가능

let num: number = 10;
num = unknownVar; // error!
```

unknown 타입의 값은 특정 타입의 변수에 할당할 수 없다.

이 외에도 연산, toUpperCase() 등의 메서드 사용도 에러가 발생하여 사용이 불가하다.

### unknown 값을 활용하고 싶은 경우

```typescript
let unknownVar: unknown = 10;
let num: number = 11;

if(typeof unknownVar === "number") {
    // unknownVar의 타입이 number인 것이 명확할 때만 대입하도록 함
    num = unknownVar;
}
```

이를 타입 정제라고 한다.

## 타입이 정해지지 않았을 때 그나마 권장하는 것 : unknown

타입이 정해지지 않아서 타입 지정이 어려울 때, any보다는 unknown 사용을 권장한다.

이유는 any와 달리 unknown은 예측 불가한 값을 넣을 수는 없기 때문이다.

이를 통해 런타임에서 발생하는 에러를 피할 수 있다.

