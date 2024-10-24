# enum

> 여러가지 값들에 각각의 이름을 부여하여 열거해두고 사용하는 타입

## 예시

사용자 역할을 정해놓고 각각의 사용자에게 역할을 부여한다고 했을 때, 이를 enum으로 관리할 수 있다.

관리자: 0
사용자: 1
방문자: 2
라고 했을 때

```typescript
const user1 = {
    name: "강희원",
    role: 0, // 0: 관리자
}
// ...
```

이렇게 설정했을 때 나중에 확인하면 role의 0이 무엇인지, 1이 무엇인지 파악이 어렵다.

이를 해결하기 위해 enum을 사용한다.

```typescript
enum Role {
    ADMIN = 0,
    USER = 1,
    GUEST = 2,
}

const user1 = {
    name: "강희원",
    role: Role.ADMIN,
}
const user2 = {
    name: "강희투",
    role: Role.USER,
}
const user3 = {
    name: "강희쓰리",
    role: Role.GUEST
}
```

## 심화

- 0,1,2... 순서대로 값을 지정해줄거라면 생략할 수 있다. 자동으로 0부터 값을 부여한다.

```typescript
enum Role {
    ADMIN, // 0
    USER, // 1
    GUEST, // 2
}
```

- 10(특정 숫자)부터 자동으로 시작하는 것도 가능하다.

```typescript
enum Role {
    ADMIN = 10, // 10
    USER, // 11
    GUEST, // 12
}
```

- 0부터 카운팅하다가 중간에 바꾸어 카운팅할 수도 있다.

```typescript
enum Role {
    ADMIN, // 0
    USER = 12, // 12
    GUEST, // 13
}
```

### 원래 컴파일하면 타입스크립트 코드는 사라지지 않나?

**enum은 컴파일 결과가 사라지지 않는다.**

컴파일 실행 시 enum은 자바스크립트의 객체로 변환되기 때문에 코드에서 값처럼 사용할 수 있다.

아래는 컴파일된 코드이다.

```javascript
var Role;
(function (Role) {
    Role[Role["ADMIN"] = 0] = "ADMIN";
    Role[Role["USER"] = 12] = "USER";
    Role[Role["GUEST"] = 13] = "GUEST";
})(Role || (Role = {}));
var Language;
(function (Language) {
    Language["korean"] = "ko";
    Language["english"] = "en";
})(Language || (Language = {}));
const user1 = {
    name: "강희원",
    role: Role.ADMIN,
    language: Language.korean
};
```

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
