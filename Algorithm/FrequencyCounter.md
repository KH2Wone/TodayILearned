# Anagram with Frequency Counter

**빈도수 세기 패턴** (Frequency Counter)을 먼저 간략히 설명하면, 객체로 각 값을 key로 만들어 value에 카운팅한 값을 저장해 카운팅 개수를 비교하여 동일한지 비교하는 것이다.

## 예시

```javascript
let obj1 = {
    a: 1,
    b: 5,
    c: 2
}
let obj2 = {
    a: 3,
    b: 5,
    c: 2,
}

// 이후 반복문을 통해 해당 스펠링의 카운트가 동일한지 확인할 수 있다
```

다른 배열과 같은지 비교 및 확인해야하는 경우 유용하게 사용할 수 있다.

for문을 작성한다고 O(n)이 아니라, 그 안에서 indexOf 등의 메서드를 사용했을 때 O(n^2)가 될 수 있다는 것을 항상 고려해야 한다.

빈도수 세기 패턴을 통해 O(n^2)를 O(n)으로 바꿀 수 있다.

**중첩 반복문 O(n^2)보다 여러개의 반복문 O(5n or 10n...)이 훨배 낫다!**

## 직접 풀이한 내용

```javascript
function validAnagram(str1, str2){
    // 글자의 길이가 다르면 false  
    if(str1.length !== str2.length) return false;

    // for 반복문을 통해 각 단어들을 객체에 담음, 두개의 문자열을 각각 객체 안에 담아서 카운팅함
      let obj1 = {}, obj2 = {};
      for(let spell of str1) {
        obj1[spell] = obj1[spell] > 0 ? ++obj1[spell] : 1;
      }
      for(let spell of str2) {
        obj2[spell] = obj2[spell] > 0 ? ++obj2[spell] : 1;
      }
    
    // 만약 obj1에 있는 값이 2에 key값과 동일하게 카운팅이 되어있다면 true
    // 아니라면 false
    for(let val in obj1) {
        if(obj1[val] !== obj2[val]) return false;
    }

    return true;
}

validAnagram('', '') // true
validAnagram('aaz', 'zza') // false
validAnagram('anagram', 'nagaram') // true
validAnagram("rat","car") // false) // false
validAnagram('awesome', 'awesom') // false
validAnagram('amanaplanacanalpanama', 'acanalmanplanpamana') // false
validAnagram('qwerty', 'qeywrt') // true
validAnagram('texttwisttime', 'timetwisttext') // true
```

강의에서 절차를 알려준대로 무작정 코드를 작성하는 것이 아니라 다양한 변수와 문제점, input 및 output 등을 고려한 후 주석으로 의사코드를 작성했다.

이렇게 하니 좀 더 체계적으로 풀이가 되는 느낌이다.

## 더 나은 방법

```javascript
function validAnagram(first, second){
    if(first.length !== second.length) return false;

    const lookup = {};

    for(let i=0; i<first.length; i++) {
        let letter = first[i];
        lookup[letter] ? lookup[letter] +=1 : lookup[letter] = 1;
    }

    for(let i=0; i<second.length; i++) {
        let letter = second[i];
        if(!lookup[letter]) {
            return false;
        } else {
            lookup[letter] -= 1;
        }
    }

    return true;
}
```

하나의 객체 안에 첫번째 문자열을 객체로 카운팅해서 담은 후 두번째 문자열을 반복문 돌면서 존재한다면 -1을 해주고, 존재하지 않는다면(-1로 인해 0이 되었거나 아예 존재하지 않는 스펠링일 경우) false를 return 하도록 한다.

만약 모든게 다 0이 되었다면 문제 없이 반복문이 종료된 후 true를 반환한다.

모든 요소가 0인지 검사할 필요도 없고 객체도 하나면 충분하고 서로 다른 값이 존재하는지 확인해줄 필요도 없는 좋은 코드다!

## 참고 및 출처

- [【한글자막】 JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/?couponCode=ACCAGE0923)
