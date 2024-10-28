# 분할과 정복 패턴

> 배열이나 문자열 같은 큰 규모의 데이터셋을 처리
> 
> 배열을 작은 조각으로 세분하여 각 조각들을 어디로 이동시킬지 결정

## 예시: 특정 값이 있는 Index 위치 찾기

### 기대값

정렬된 상태의 배열을 받는다고 가정한다.

```javascript
search([1,2,3,4,5,6], 4) // 3
search([1,2,3,4,5,6], 6) // 5
search([1,2,3,4,5,6], 11) // -1
```

### 선형탐색 풀이

```javascript
function search(arr, val) {
    for (let i=0; i<arr.length; i++) {
        if(arr[i] === val) {
            return i;
        }
    }
    return -1;
}
```

해당 구조를 선형 탐색이라고 한다.

이 형태로 가면 arr이 엄청 많을 때 느릴 것이다.
이를 위해 정렬된 배열로 받는다고 했을 때, 이진 탐색을 취한다.

### 이진탐색 풀이

```javascript
function search(array, val) {
    let min = 0; // 조각 시작 지점
    let max = array.length - 1; // 조각 끝 지점
    
    while(min <= max) { // max 만큼만 반복문이 돌아야함
        let middle = Math.floor((min + max) / 2);
        
        if(array[middle] < val) {
            min = middle + 1;
        }
        else if(array[middle] > val) {
            max = middle - 1;
        }
        else {
            return middle;
        }
    }
    return -1;
}
```

중간 지점의 값을 찾으려는 값과 비교하여 큰지 작은지 확인하고 작다면 왼쪽 부분 배열은 무시한다.

오른쪽 부분의 배열에서 또 가운데 값을 찾고, 비교하여 왼쪽 혹은 오른쪽을 무시하고를 반복하면 빠르게 찾을 수 있다.

## 참고 및 출처

- [【한글자막】 JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/?couponCode=ACCAGE0923)
