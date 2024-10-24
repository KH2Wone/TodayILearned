# tsconfig.json

## tsconifg 쓰는 이유

컴파일러 옵션을 얼마나 엄격하게 타입 오류를 검사할건지, 자바스크립트 코드의 버전은 어떻게 할건지 등을 옵션으로 설정할 수 있다.

## 시작하기에 앞서

ts --init을 하면 tsconfig.json 파일이 만들어진다.

> 컴파일 옵션은 Node.js 패키지 단위로 설정이 가능

## 옵션 구성

### include :: 여기부터 여기까지 다 컴파일 해주세요

```json
{
    "include": ["src"] // src 안에 있는 모든 파일을 한번에 컴파일
}
```

.ts 파일을 컴파일할 때 `tsc src/index.ts` 이런식으로 실행할 수 있는데 수 많은 typescript 파일이 있다고 할 때 특정 폴더 안에 있는 파일들을 자동으로 컴파일되게 설정할 수 있다.

`tsc`를 터미널에 입력하면 src에 있는 모든 파일이 바로 컴파일 되는 것을 확인할 수 있다.


### compilerOptions - target :: 컴파일된 JavaScript의 버전 설정

```json
{
    // compilerOptions: 타입 검사, 컴파일하는 과정 등 상세 옵션
    "compilerOptions": {
        "target": "ESNext" // 가장 최신 버전
    }
}
```

예를 들어, ES5 설정 시 화살표 함수를 사용하게 되면 컴파일된 자바스크립트 파일에는 함수표현식으로 바뀐다.

> 타입스크립트로 만든 프로젝트가 ES6를 지원하지 않는 곳에서 동작할 수 있기에 이를 방지하기 위함 
> 
> (구형 버전, 예전 서버 환경 등)

### compilerOptions - module :: 자바스크립트 코드의 모듈 시스템 설정

```json
{
    "compilerOptions": {
        "module": "CommonJS"
    }
}
```

target 옵션과 같이 프로젝트 환경 및 상황에 따라 모듈 옵션을 조정할 수 있다.

> 타입스크립트로 만든 프로젝트가 ES6를 지원하지 않는 곳에서 동작할 수 있기에 이를 방지하기 위함 
> 
> (구형 버전, 예전 서버 환경 등)

### compilerOptions - outDir :: 컴파일된 파일(js) 어디에 다 저장할래?

```json
{
    "compilerOptions": {
        "outDir": "dist" // 최상위 위치에 dist 라는 폴더 안에 저장된다
    }
}
```

컴파일된 자바스크립트 파일을 한곳에 분류해둘 수 있는 방법이다.

### compilerOptions - strict :: 컴파일러가 타입을 검사할 때 얼마나 엄격할지

```json
{
    "compilerOptions": {
        "strict": true
    }
}
```

`strict`가 true일 때 엄격하게 타입을 검사한다.

예시)

```typescript
const test = (message) => { // 매개변수 Error
    console.log("Hello" + message)
}
```

> 매개변수는 타입스크립트가 추론할 수 없기 때문에 프로그래머가 직접 타입을 지정하도록 권장

false는 오류가 사라진다.

자바스크립트 코드를 타입스크립트 코드로 마이그레이션 하는 경우에는 `strict`를 해제하는 것이 낫다.

### compilerOptions - moduleDetection :: 자동으로 각 파일의 스코프 지정하기

설명에 앞서 자바스크립트는 각각의 파일이 개별 모듈로 취급받는다.

그러나 타입스크립트는 모든 파일을 전역모듈로 보기 때문에 파일이 분리되어 있어도 한 공간에 있다고 취급된다.

```typescript
// test.ts
const a = 1; // Error

// test2.ts
const a = 2; // Error
```

동일한 이름의 변수를 똑같은 스코프에 두번 정의했다고 에러가 발생한다.

#### (tsconfig.json 없이) 해결법

각 파일에 `export {};` 를 넣어주면 해당 파일은 독립된 공간으로 인식된다.

```typescript
// test.ts
const a = 1;
export {};

// test2.ts
const a = 2;
export {};
```

#### tsconfig.json을 통한 쉬운 해결법

```json
{
    "compilerOptions": {
        "moduleDetection": "force"
    }
}
```

다음과 같이 설정하면 에러가 사라지고 컴파일된 파일에서 `export {};`가 하단에 추가된 것을 확인할 수 있다.

> **export {};인 이유?**
>
> compilerOptions > module 의 옵션을 ESNext로 설정하면 그렇다.
> 
> CommonJS로 설정하면 exports같은 CommonJS 시스템으로 맞추어 컴파일 된다.


### ts-node - esm :: ts-node를 ES module 시스템으로 동작하게 하기

타입스크립트 파일을 한번에 실행할 수 있게 해주는 ts-node (tsx도 가능)

만약 compilerOptions > module 값을 ESNext로 설정한 후, ts-node를 실행하면 에러가 발생한다.

ts-node가 ES module을 이해하지 못했기 때문이다. (export)

ts-node는 기본적으로 CommonJS를 사용하기에 ES module 시스템을 이해할 수 없다.

```json
{
    "ts-node": {
        "esm": true
    }
}
```

true로 설정해주면 ts-node가 ES module 시스템으로 동작하게 된다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
