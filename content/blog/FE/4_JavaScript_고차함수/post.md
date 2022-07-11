---
title: 'JavaScript, 화살표 함수와 고차함수'
date: 2022-07-10 20:10:13
category: 'FE'
draft: false
---

<!-- <p align="center"><img src="1.png" height="300px" width="600px"></p> -->

이번 포스팅은 JavaScript이다. JavaScript는 내용이 많기에 필요할 때마다 찾아보도록 하고, 오늘은 JavaScript의 람다식 사용법과 고차함수의 종류 대해 설명하겠다.

### JavaScript 공부 관련 검색 사이트

- [TCP School](http://tcpschool.com/javascript/intro)
- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript)
- [W3School](https://www.w3schools.com/js/default.asp)

# 화살표 함수, 람다식

화살표 함수 표현이란 전통적인 함수표현(function으로 함수를 정의하면서 표현)의 간편한 대안이다. 일반적으로 가독성이 좋다는 장점이 있다.

기본적으로 다음과 같은 방법으로 사용한다. `(매개변수) => {함수 실행 내용}`

```js
const sum1 = (a, b) => a + b
const sum2 = (a, b) => a + b
const sum3 = (a, b) => {
  return a + b
}

console.log(sum1(1, 3))
console.log(sum2(1, 3))
console.log(sum3(1, 3))
```

다음과 같은 방법들로 화살표 함수를 정의 할 수 있으며, 매개변수가 1개일 경우, 괄호를 적지 않아도 된다.

# 고차함수

고차함수는 함수를 인자(argument로 받거나 함수를 리턴하는 함수를 말한다. 이때 다른 함수를 인자로 전달되는 함수를 콜백함수라고 한다. 고차함수는 외부 상태 변경이나 가변데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다.

## CallBack 콜백함수

콜백함수란 이름 그대로 나중에 호출되는 함수를 말한다. 어떤 이벤트가 달성했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수를 말한다. 따라서 콜백함수는 유니크한 문법적 특징을 가지고 있는 것이 아니라, 호출 방식에 의한 구분이다.

## Filter 함수

Filter 함수는 특정 조건을 만족하는 새로운 배열을 필요로 할때 주로 사용한다.

```jsx
var newArray = arr.filter(callbackFunction [, this.Arg]);
// 다음과 같은 방법으로 선언한다.
// 예시
const numbers = [1, 2, 3, 4, 5];
const result = numbers.filter(number => number>3);

console.log(numbers);
// [1,2,3,4,5]
console.log(result);
// [4,5]
```

위에 filter함수를 보면 콜백함수의 조건을 만족시키는 경우에 해당 값만 return하여 원하는 데이터만 추출하여 새로운 배열을 만들 수 있다.

## Map 함수

배열의 요소를 일괄적으로 변경하는데 효과적이다. 즉 각 배열에 콜백함수를 적용시켜 배열의 요소들을 변경시킨 배열을 return 해준다.

- `배열.map(콜백함수)`;
- `배열.map((요소, 인덱스, 배열) => {return 요소}`;

```jsx
const multiply2 = num => num * 2
const arr = [1, 2, 3, 4, 5]

console.log(arr.map(multiply(2)))
// [2,4,6,8,10]

// 아래 내용은 arr.map(multiply(2))와 같다 단 콜백함수를 그냥 정의해준 것이다.
arr.map((el, idx, arr) => {
  return el * 2
})
```

결과 값을 보면 2,4,6,8,10 을 리턴해주었는데, 즉 arr의 각 요소에 2를 곱해준 값을 return 해 준것과 같다.

## reduce 함수

reduce함수는 각 배열의 요소에 누적연산을 해주는 함수이다. 즉 reduce함수는 배열의 각 요소에 대해 리듀서(reducer)함수를 실행한다.

리듀서 함수(리듀스함수의 콜백함수)는 네게의 인자를 가진다

1. accumulator - accumulator는 callback함수의 반환값을 누적합니다.
2. currentValue - 배열의 현재 요소
3. index(Optional) - 배열의 현재 요소의 인덱스
4. array(Optional) - 호출한 배열

또한 reduce함수에서 initialValue는 최초 callback함수 실행시 accumulator 인수에 제공되는 값이다. 선택으로 사용할 수 있으므로, 초기값을 제공하지 않을 경우 배열의 첫번째 요소를 사용한다.

```jsx
arr.reduce((accumulator, currrentValue, index, array)=> { return 어떤 연산}, initialValue);
```

- 위 함수에 반환값은 배열을 각 순서대로 불러 각 요소에 대한 callback함수를 실행한 결과를 누적한 값이다.
  예제

```jsx
const arr = [1, 2, 3, 4, 5]
const value = arr.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  0
)
console.log(value)
```

위의 예제는 초기값이 0이고, 1,2,3,4,5를 다 더한 값을 출력하므로 15가 출력된다.

```jsx
const arr = [1, 2, 3, 4, 5]
const value = arr.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  -1
)
console.log(value)
```

마찬가지로 위에 예제는 초기값을 -1로 두었기 때문에 출력값은 14가 된다.

# Reference

reduce 함수 예제 : https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
