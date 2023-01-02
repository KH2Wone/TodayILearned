# JSX에서 줄바꿈이 되지 않는 문제 (개행 처리)

```javascript
const DESCRIPTION = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.\nDuis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.';
```

이렇게 리액트에서 상수로 해당 문장을 저장해놓고 꺼내 써야 한다고 했을 때 개행 처리한 부분이 줄바꿈이 되지 않는 것을 확인할 수 있습니다.
개행 문자 `\n` 가 동작하지 않는 것입니다.

`<br />`을 사용해도 동일합니다.

어떤 문제인지 계속 찾아보다보니, 리액트에서만 유효하게 발생되는 문제라고 파악되어 구글링해보았습니다.

<br>

## white-space

> [[MDN] white-space](https://developer.mozilla.org/ko/docs/Web/CSS/white-space)

white-space라는 CSS 속성을 사용하면 됩니다.

개행 문자에서 줄바꿈이 자동으로 일어난다는 속성들을 확인해보면,

```
pre-wrap
연속 공백 유지. 줄 바꿈은 개행 문자와 <br> 요소에서 일어나며, 한 줄이 너무 길어서 넘칠 경우 자동으로 줄을 바꿉니다.

pre-line
연속 공백을 하나로 합침. 줄바꿈은 개행 문자와 <br> 요소에서 일어나며, 한 줄이 너무 길어서 넘칠 경우 자동으로 줄을 바꿉니다.

break-spaces
다음 차이점을 제외하면 pre-wrap과 동일합니다.

연속 공백이 줄의 끝에 위치하더라도 공간을 차지합니다.
연속 공백의 중간과 끝에서도 자동으로 줄을 바꿀 수 있습니다.
유지한 연속 공백은 pre-wrap과 달리 요소 바깥으로 넘치지 않으며, 공간도 차지하므로 박스의 본질 크기(min-content, max-content)에 영향을 줍니다.
```

이렇게 세 가지가 있습니다.

여기서 가장 흔히 사용되고, 단순 줄바꿈을 위한 사용이라면 `white-space: pre-line`가 좋습니다.

반복되는 스페이스, 탭을 하나로 합쳐 처리해주기 때문입니다.

