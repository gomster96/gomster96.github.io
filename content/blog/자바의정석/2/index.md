---
title: '[자바의 정석] 2장 변수 variable'
date: 2021-12-23 16:22:13
category: '자바의 정석'
draft: false
---

# 변수란?

변수란 단 하나의 값을 저장할 수 있는 메모리 공간 이다.

- 단 하나의 값만 저장할 수 있으므로 새로운 값을 저장하면 기존의 값은 사라진다.

# 변수의 타입

## 기본형 (Primitive type)

- 논리
  - boolean - 1byte
  - true, false
    1bit로 충분하지만 자바에서 데이터를 다루는 최소단위가 byte이기 때문에 boolean의 크기는 1byte이다.
- 문자
  - char - 2byte
- 정수
  - byte - 1byte
  - short - 2byte
  - int - 4byte
  - long - 8byte
    int 는 CPU가 가장 효율적으로 처리할 수 있는 타입이기 때문에 엥간하면 int를 사용하고, 효율적인 실행보다 메모리 절약이 우선이면 byte나 short를 사용해야한다.
- 실수
  - float - 4byte
  - double - 8byte

## 참조형

- 객체의 주소를 저장한다.

```java
클래스이름 변수이름; // 이런 방법으로 선언한다. 변수의 타입이 기본형이 아니면 다 참조형이다.
```

# 상수와 리터럴

## 상수

상수는 앞에 final 이라는 키워드를 붙여주며, 상수의 이름은 모두 대문자로 하는것이 관례이다

반드시 선언과 동시에 초기화 해야하며, 그 이후부터 상수의 값을 변경할 수 없다.

```java
final int MY_CONSTANT = 10;
```

## 리터럴

1, 12 , 'A' 등 그 자체로 값을 의미하는 것을 뜻한다.

# 입력받기 - Scanner

```java
import java.util.Scanner;
Scanner sc = new Scanner(System.in);
```

다음과 같이 import 한 이후 객체를 생성해서 사용할 수 있다.

```java
int data = sc.nextInt(); // data에 다음 입력받는 int를 저장한다
String data = sc.nextLine(); // data에 다음 입력하는 라인을 저장한다.
```

# Reference

- 남궁성, Java의 정석 (3rd Edition), 도우출판
