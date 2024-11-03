# 재귀

> 자기 자신 (함수)를 반복하여 호출

## 재귀의 잠재적 위험

1. 종료 조건이 없을 때
2. 값을 반환하지 않거나 잘못된 것을 반환하는 것

## Helper 메소드 재귀

```javascript
function collectOddValues(arr) {
    let result = []; // 재귀 함수 내에서 정의하면 계속 빈 배열로 초기화됨
    
    function helper(helperInput) {
        if(helperInput.length === 0) return;
        
        if(helperInput[0] % 2 !== 0) {
            result.push(helperInput[0]);
        }
        
        helper(helperInput.slice(1));
    }
    helper(arr);
    
    return result;
}
```

헬퍼 메소드가 다른 함수 내부에 정의되어 있고 아래에서 호출한다.

재귀 함수를 감싸는 정말 헬퍼 역할을 하는 collectOddValues가 헬퍼 메소드 재귀이다.

result도 재귀함수 바깥에 있는 것을 확인할 수 있다.

## 순수 재귀

```javascript
function collectOddValues(arr) {
    let newArr = [];
    
    if(arr.length === 0) return newArr;
    if(arr[0] % 2 !== 0) {
        newArr.push(arr[0]);
    }
    
    newArr = newArr.concat(collectOddValues(arr.slice(1)));
    return newArr;
}
```

이렇게 순수 재귀 함수를 만들고 싶은 경우 slice, spread operator, concat 등의 메소드를 사용하면 배열을 변경할 필요가 없어서
헬퍼 메소드가 필요 없다.

문자열은 변경할 수 없기 때문에 slice 혹은 substring을 사용해서 복사본을 만들어야 한다.

## 참고 및 출처

- [【한글자막】 JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/?couponCode=ACCAGE0923)
