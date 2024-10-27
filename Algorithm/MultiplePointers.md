# 다중 포인터 패턴

> 배열의 각 양 끝에서부터 가운데로 옮겨가며 두 값을 비교
> 
> 포인터가 두개라는 것이 중요하지, 가운데로 서서히 옮겨가는지 혹은 왼쪽부터 함께 옮겨가는지는 조건에 따라 다르다.

## 예시: 두 개의 값을 합 했을 때 0이 되는 값들 구하기

### 기대값
```javascript
sumZero([-3, -2, -1, 0, 1, 2, 3]) // [-3, 3]
sumZero([-2, 0, 1, 3]) // undefined
sumZero([1, 2, 3]) // undefined
```

정렬되어 있다고 가정한다.

### 풀이

```javascript
function sumZero(arr) {
    let left = 0; // 첫번째 값
    let right = arr.length - 1; // 맨 마지막 값
    
    while(left < right) {
        let sum = arr[left] + arr[right];
        if(sum === 0) { // 더한 값이 0이면 배열에 담아 return, [-3, 3] 형태
            return [arr[left], arr[right]];
        } else if(sum > 0) { // 0보다 크다면 오른쪽의 포인터를 옮김
            right--;
        } else { // 0보다 작다면 왼쪽의 포인터를 옮김
            left++;
        }
    }
}
```

원하는 값을 찾을 때까지 두 개의 포인터를 조건에 따라 이동시키는 방법이다.

## 예시: 고유한 숫자 개수 세기

### 기대값

```javascript
countUniqueValues([1, 1, 1, 1, 1, 2]) // 2
countUniqueValues([1, 2, 3, 4, 4, 7, 12, 12, 13]) // 7
countUniqueValues([]) // 0
countUniqueValues([-2, -1, -1, 0, 1]) // 4
```

이 또한 정렬되어 있다고 가정한다.

### 나의 풀이

```javascript
function countUniqueValues(arr) {
    if(arr.length < 1) return 0;
    let i = 0;1
    let j = 1;

    // j가 arr의 길이만큼 도달했다면 반복문을 종료한다
    while(j < arr.length) {
        // i와 j를 비교하여 같은지 확인
        // 같으면 j를 +1
        // 다르면 i를 +1 해주고 해당 자리의 값을 현재 j로 변경
        if(arr[i] === arr[j]) {
            j++;
        }
        else {
            ++i;
            arr[i] = arr[j];
        }
    }
    // return은 i + 1
    return i + 1;
}
```

### 더 나은 풀이

```javascript
function countUniqueValues(arr) {
    if(arr.length === 0) return 0;
    var i = 0;
    for(var j = 0; j < arr.length; j++) {
        if(arr[i] !== arr[j]) {
            i++;
            arr[i] = arr[j];
        }
    }
    return i + 1;
}
```

코드가 훨씬 간결하다..!

값이 동일하지 않을 때에만 고려하고 for문으로 작성하면 된다.

그래도 설명만 듣고 끝까지 풀어볼 수 있어서 다행이었다.

## 참고 및 출처

- [【한글자막】 JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/?couponCode=ACCAGE0923)
