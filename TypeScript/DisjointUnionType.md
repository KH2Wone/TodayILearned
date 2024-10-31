# 서로소 유니온 타입

> 교집합이 없는 타입들로만 만든 유니온 타입

## 예시1

```typescript
type Admin = {
    name: string;
    kickCount: number;
}
type Member = {
    name: string;
    point: number;
}
type Guest = {
    name: string;
    visitCount: number;
}
type User = Admin | Member | Guest;

function login(user: User) {
  if('kickCount' in user) {
      // admin type
      console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴했습니다.`);
  }  else if ('point' in user) {
      // member type
      console.log(`${user.name}님 현재까지 ${user.point}모았습니다.`);
  } else {
      // guest type
      console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다.`);
  }
};
```

만약 전에 배운 것 처럼 이렇게 접근한다면, 좋은 코드는 아닐 것이다.

왜냐하면 해당 조건문으로는 저 조건이 Admin 조건인지, Member 조건인지, Guest 조건인지 코드를 처음보는 사람은 이해할 수 없다.

이를 위해 **서로소 유니온 타입**을 적용할 수 있다.

```typescript
type Admin = {
    tag: "ADMIN"; // add
    name: string;
    kickCount: number;
}
type Member = {
    tag: "MEMBER"; // add
    name: string;
    point: number;
}
type Guest = {
    tag: "GUEST"; // add
    name: string;
    visitCount: number;
}
type User = Admin | Member | Guest; // 서로소 유니온 타입

function login(user: User) {
    switch (user.tag) {
        case "ADMIN": {
            console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴했습니다.`);
            break;
        }
        case "MEMBER": {
            console.log(`${user.name}님 현재까지 ${user.point}모았습니다.`);
            break;
        }
        case "GUEST": {
            console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다.`);
            break;
        }
    }
};
```

집합으로 보았을 때 이전 코드에서는 교집합이 존재할 가능성이 있었다.

예를 들면, name, point, visitCount 요소를 모두 가진 객체가 있을 수 있기 때문에 (교집합)
혼선이 있을 수 있다.

이를 해결하기 위해 타입에 string 리터럴 타입의 tag 요소를 추가했다.

아무런 교집합도 없는 서로소 집합의 관계로 변한다.
즉, Admin 타입이면서 Member 타입인 경우가 존재하지 않는 것이다.

이후 코드처럼 작성하면 가독성이 있어 알아보기 쉬우며 직관적으로 타입을 좁힐 수 있다.

## 예시2

비동기 작업의 결과를 처리하는 객체도 서로소 유니온 타입으로 안전하게 작성할 수 있다.

```typescript
type LoadingTask = {
    state: "LOADING";
}
type FailedTask = {
    state: "FAILED";
    error: {
        message: string;
    }
}
type SuccessTask = {
    state: "SUCCESS";
    response: {
        data: string;
    }
}
type AsyncTask = LoadingTask | FailedTask | SuccessTask;

function processResult(task: AsyncTask) {
    switch (task.state) {
        case "LOADING": {
            console.log('로딩중');
            break;
        }
        case "FAILED": {
            console.log(`에러 발생 : ${task.error.message}`);
            break;
        }
        case "SUCCESS": {
            console.log(`성공 : ${task.response.data}`);
            break;
        }
    }
}
```

state로 타입을 좁히고, `task.error` 혹은 `task.response` 뒤에 ? 이나 ! 연산자로 값이 존재한다고 단언하거나 있을 경우라고 불안정하게 표현해주지 않아도 된다는 장점이 있다.

왜냐하면 state로 우선 타입을 좁혔다면 그 안에 FAILED일 경우 `task.error.message`는 반드시 존재하고,
SUCCESS일 경우 `task.response.data` 또한 반드시 존재할 것이기 때문이다.

이렇게 동시에 여러가지 상태를 표현해야하는 경우 선택적 프로퍼티를 사용하는 것 보다는 상태에 따라 타입을 잘게 쪼개는 것이 도움된다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
