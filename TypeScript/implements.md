# implements

> 클래스에서 타입스크립트를 사용하는 방법
>
> implements A: A의 타입으로 조건을 강제

## implements

```typescript
interface CharacterInterface {
    name: string;
    moveSpeed: number;

    move(): void;
}

class Character implements CharacterInterface {
    // implements의 경우 constructor 안에 private or protected는 불가
    constructor(public name: string, public moveSpeed: number) {
    }

    move(): void {
        console.log(`${this.moveSpeed} 속도로 이동!`)
    }
}
```

새로운 클래스의 설계도를 미리 그려놓는 역할을 한다.

interface의 값들이 필수로 들어가야 에러가 발생하지 않는다.

라이브러리의 구현이나 복잡하고 정교한 프로그래밍을 위해서는 설계도를 구현하는 과정이 필요할 수 있기에
그때 사용하면 좋다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
