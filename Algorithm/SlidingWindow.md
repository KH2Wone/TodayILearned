# 기준점 간 이동 배열 패턴

> 창문을 왼쪽에서 오른쪽 혹은 오른쪽에서 왼쪽으로 바꿔 끼우는 것

## 예시: 정해진 길이만큼의 숫자를 더하여 가장 큰 값 찾기

### 기대값

```javascript
maxSubarraySum([1,2,5,2,8,1,5], 2) // 10
maxSubarraySum([1,2,5,2,8,1,5], 4) // 17
maxSubarraySum([4,2,1,6,2], 4) // 13
maxSubarraySum([], 4) // null
```

### 풀이

```javascript
function maxSubarraySum(arr, num) {
    let maxSum = 0;
    let tempSum = 0;
    if(arr.length < num) return null; // 유효하지 않은 배열 처리
    
    // 첫 num개의 숫자를 합산하여 maxSum에 담는다
    for (let i=0; i<num; i++) {
        maxSum += arr[i];
    }
    tempSum = maxSum; // tempSum에 첫 max 값을 넣어준다
    // 위에서 이미 첫 합산값을 계산했으므로 num 위치에서 시작
    // 즉, num 위치에서 오른쪽으로 num개의 요소를 더하는게 아니라 왼쪽으로 더함
    for (let i=num; i<arr.length; i++) {
        tempSum = tempSum - arr[i-num] + arr[i];
        maxSum = Math.max(maxSum, tempSum);
    }
    return maxSum;
};
```
([1,2,5,2,8,1,5], 2) 라는 매개변수가 주어졌을 때

[1,2,5,2,8,1,5]

1, 2 를 묶어주고

[1,2,5,2,8,1,5]

2, 5를 묶어주는데 이 때 그저 1을 가리키던 방향이 5로 가면 되는 것이다.

하나하나 다시 계산할 필요 없다.

합산한 값에서 앞쪽 index 위치만 하나씩 바꿔가면서 더해주고 빼주면 된다.

## 참고 및 출처

- [【한글자막】 JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/?couponCode=ACCAGE0923)
