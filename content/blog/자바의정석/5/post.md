---
title: '[자바의 정석] 5장 배열 Array'
date: 2021-12-23 16:25:13
category: '자바의 정석'
draft: false
---

# 배열이란?

배열은 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것을 말한다.

```java
타입[] 변수이름 = new 타입[길이];
int[] score = new int [5];
```

- 'new' 에 의해서 메모리의 빈 공간에 5개의 int형 데이터를 저장할 수 있는 공간이 마련된다.

## 배열이름.length

- 생성된 배열의 길이를 반환한다.
- 배열은 한번 생성하면 길이를 **변경할 수 없기 때문에**, 이미 생성된 배열의 길이는 변하지 않는다.
  -> '배열이름.length'는 상수이다

## 배열의 길이 변경하기

배열은 한번 선언되고나면 길이를 변경할 수 없다. 따라서 새로운 배열을 생성한 뒤에 값을 복사해야한다.

1. 더 큰 배열을 새로 생성한다.
2. 기존 배열의 내용을 새로운 배열에 복사한다.

### 배열 복사

- 배열을 복사할 때 for문으로 하나하나 복사할 수도 있지만 System.arraycopy() 함수를 사용하여 복사할 수 있다.

```java
System.arraycopy(num, 0, newNum, 0, num.length)
// num이라는 배열의 0번째 index부터 num.length개의 데이터를 newNum의 0번째부터 복사한다.
System.arraycopy(num, 1, newNum, 2, 5)
// num이라는 배열의 1번째 index부터 5개의 데이터를 newNum의 2번째부터 복사한다.
```

## 배열의 출력

배열을 출력하기 위해선 다음의 방법들이 있다.

- for문으로 하나하나 요소들을 출력하기
- Array.toString(배열이름) 메서드 사용하기
  -> [1번째 요소, 2번째 요소, ...] 과 같은 꼴로 출력된다.

### 참고 : 변수 타입의 기본값

- boolean : false
- char : `\u0000`
- byte, short, int : 0
- long : 0L
- float : 0.0f
- double 0.0d 또는 0.0
- 참조형 변수 (String, 그외 객체) : null

# String 배열

- 따라서 String도 참조형 변수이므로 배열로 선언할 경우 기본값은 null이다.
- String클래스는 char배열에 메소드를 추가함으로써 구현하였다.

### String의 메서드

```java
char charAt(int index) // 문자열에서 해당 index에 있는 문자를 반환한다.

int length() // 문자열의 길이를 반환한다

String substring(int from, int to) // 문자열에서 해당 범위(from~to)에 있는 문자열을 반환한다.
//문자열 일부 추출

boolean equals(Object obj) // 문자열의 내용이 obj와 같은지 확인한다

char[] toCharArray // 문자열을 char array로 변환하여 반환한다.
```

# 2차원 배열 선언

```java
타입[][] 변수이름 = new 타입[row][column]
```
