# 유틸리티 타입

## Partial<T>

> 특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 바꿔주는 타입

```typescript
interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
}

const draft: Partial<Post> = {
    title: "제목 나중에",
    content: "초안"
}
```

Partial 타입은 `tag` 프로퍼티가 필수 타입이지만
사용하지 않았을 때 에러를 발생하지 않고 모든 프로퍼티를
선택적 프로퍼티로 바꿔준다.

### 직접 구현해보기

```typescript
type Partial<T> = {
    [key in keyof T]?: T[key];
}
```

## Required<T>

> 특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 바꿔주는 타입

```typescript
const withThumbnailPost: Required<Post> = {
    title: "한입 타스 후기",
    tags: ["ts"],
    content: "",
    thumbnailURL: "https://..."
}
```

`thumbnailURL` 프로퍼티가 Required<Post>로 인해 필수 프로퍼티가 되어서
반드시 사용하도록 오류를 발생시켜 강제할 수 있다.

### 직접 구현해보기

```typescript
type Required<T> = {
    [key in keyof T]-?: T[key];
}
```

`-?` 이 코드를 넣어주면 모든 프로퍼티가 필수 프로퍼티가 된다.

## Readonly<T>

> 특정 객체 타입에서 모든 프로퍼티를 읽기 전용 프로퍼티로 만들어주는 타입

```typescript
const readonlyPost: Readonly<Post> = {
    title: "보호된 게시글 입니다.",
    tags: [],
    content: "",
}

readonlyPost.content = "hi"; // error!
```

### 직접 구현해보기

```typescript
type Readonly<T> = {
    readonly [key in keyof T]: T[key];
}
```

## Pick<T, K>

> 객체 타입으로부터 특정 프로퍼티만 딱 골라내는 타입

```typescript
interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
}

const legacyPost: Pick<Post, "title" | "content"> = {
    title: "옛날 글",
    content: "옛날 컨텐츠"
}
```

legacyPost 객체가 과거에 이미 존재했으나 Post 타입이
생성되었을 때 Post 타입을 지정하려면 tags 프로퍼티가 없어서 에러가 발생한다.

이를 해결하기 위해 Pick을 사용하여 필요한 프로퍼티만 꺼내
타입을 지정해줄 수 있다.

### 직접 구현해보기

```typescript
type Pick<T, K extends keyof T> = {
    [key in K]: T[key];
}
```

K에 extends keyof T를 붙여주어야 never 같은 특정 타입이 들어올 수 있는
에러를 방지한다.

그래서 keyof로 타입을 좁혀주어야 한다.

## Omit<T, K>

> 객체 타입으로부터 특정 프로퍼티를 제거하는 타입

```typescript
const noTitlePost: Omit<Post, "title"> = {
    content: "",
    tags: [],
    thumbnailURL: ""
}
```

필수 프로퍼티인 title 프로퍼티만 제거한 타입 interface를 만들어준다.

## 직접 구현해보기

```typescript
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

Pick 유틸리티 타입을 사용하였는데,

이것은 즉

Pick<Post, Exclude<keyof Post, 'title'>>
Pick<Post Exclude<'title' | 'content' | 'tags' | 'thumbnailURL', 'title'>>
Pick<Post, 'content' | 'tags' | 'thumbnailURL'>

이렇게 해석할 수 있다.

Exclude를 통해 title 프로퍼티를 제거한 후 Pick 타입으로
제거한 후의 타입을 골라내 적용했다.

## Record<T, K>

> 객체 타입을 만들어주는 유틸리티 타입

```typescript
type ThumbnailLegacy = {
    large: {
        url: string;
    };
    medium: {
        url: string;
    };
    small: {
        url: string;
    }
}

type Thumbnail = Record<"large" | "medium" | "small", { url: string }>;
```

Thumbnail 타입은 ThumbnailLegacy 타입과 동일하다.

첫번째 타입 변수는 객체의 프로퍼티 키를 유니온 타입으로 받고,
두번째 타입 변수는 이 키들의 value 타입을 받는다.

여기서 watch 타입을 추가해야한다면, 첫번째 타입 변수에 `"watch"`를 추가해주거나
value 타입이 추가되어야 한다면 두번째 타입 변수에
`size: number` 같이 넣어주면 된다.

### 직접 구현해보기

```typescript
type Record<K extends keyof any, V> = {
    [key in K]: V;
}
```

무슨 타입인지는 모르지만, 어떤 객체의 key를 추출해둔 유니온 타입이다
라는 의미로 K 뒤에 extends keyof any를 넣어준다.

## 참고 및 출처

- [한 입 크기로 잘라먹는 타입스크립트(TypeScript)](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%ED%81%AC%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8?srsltid=AfmBOoqKyeukk5UXUwfKCAc4kjJVMZ6l_1muf8wV2_i14aiBihNU4Kbs)
