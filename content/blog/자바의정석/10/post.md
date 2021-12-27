---
title: '[자바의 정석] 10장 날짜와 시간 & 형식화 date, time and formatting'
date: 2021-12-27 20:28:00
category: '자바의 정석'
draft: false
---

# Calendar와 Date

Calender와 Date는 자바 탄생부터 지금까지 20년 넘게 사용되는 패키지이다. 단 최근에는 새로 추가된 java.time 패키지로 기존의 모든 단점들이 개선되어 주로 사용된다.

## Calendar

캘렌더는 추상클래스이기 때문에 직접 개체를 생성할 수 없고, 메서드를 통해(getInstance) 완전 구현된 클래스의 인스턴스를 얻어야한다.

```java
Calender cal = new Calendar(); // 에러 발생! 추상클래스이기 때문에 인스턴스를 만들 수 없다.
// 대신에 아래와 같이 사용한다.
Calender cal = Calender.getInstance();
```

### Date와 Calendar 간의 변환

1. Calendar 를 Date로 변환

```java
	Calendar cal = Calendar.getInstance();
	// 아래와 같이 Date객체를 만들 수 있다.
	Date d = new Date(cal.getTimeInMills()); //Date(long date)로 새롭게 Date 객체 생성
```

2. Date를 Calendar 로 변환

```java
Date d = new Date();

Calendar cal = Calendar.getInstance();
cal.setTime(d);
```

## Calender 사용

- getInstance()를 통해서 얻은 인스턴스는 기본적으로 현재 시스템의 날짜와 시간에 대한 정보를 가지고 있기 때문에 이를 원하는 날짜나 시간으로 설정하려면 set메서드를 사용하고, 원하는 값을 가져오려면 get메서드를 사용한다.
- get(Calendar.MONTH)로 얻어오는 값의 범위가 1~12가 아니라 **0~11이다**. 즉 0이면 1월, 1이면 2월을 뜻한다.

```java
import java.util.*;

class CalendarEx1{
	public static void main(String[] args){
		// 기본적으로 현재 날짜와 시간으로 설정된다
		Calendar today = Calendar.getInstace();

		System.out.println("이 해의 년도 : " + today.get(Calendar.YEAR));
		System.out.println("월(0~11, 0:1월) : " + today.get(Calendar.MONTH));
		System.out.println("이 해의 몇째주 : " + today.get(Calendar.WEEK_OF_YEAR));
		System.out.println("이 달의 몇째주 : " + today.get(Calendar.WEEK_OF_MONTH));

		System.out.println("이 달의 몇 일 : " + today.get(Calendar.Date));
		System.out.println("이 달의 몇 일 : " + today.get(Calendar.DAY_OF_MONTH));
		System.out.println("이 해의 몇 일 : " + today.get(Calendar.DAY_OF_YEAR));
		System.out.println("요일(1~7), 1:일요일 : " + today.get(Calendar.Day_OF_WEEK));
		System.out.println("이 달의 몇번 째 요일 : " + today.get(Calendar.DAY_OF_WEEK_IN_MONTH));
		// ex) 이달의 4번째 월요일 : 4 출력
		System.out.println("오전_오후(0:오전, 1:오후) : " + today.get(Calendar.AM_PM));

		System.out.println("시간(0~11): " + today.get(Calender.HOUR));
		System.out.println("시간(0~23): " + today.get(Calender.HOUR_OF_DAY));
		System.out.println("분(0~59): " + today.get(Calender.MINUTE));
		System.out.println("초(0~59): " + today.get(Calender.SECOND));
		System.out.println("밀리초(0~999): " + today.get(Calender.MILLISECOND));

		System.out.println("이달의 마지막 날 : " + today.getActualMaximum(Calendar.DATE)) ;
		// 이달의 마지막 일을 찾는다
	}
}
```

두 날짜간의 차이를 구하기 위해서는 두 날짜를 최소단위인 밀리초단위(Millisecond)로 변경한 다음 그 차이를 구하면 된다.

- getTimeInMillis()는 1/1000초 단위로 값을 반환하기 때문에 초단위로 얻기 위해서는 1000으로 나눠줘야한다.
- 일단위로 얻기 위해서는 24 _ 60 _ 60 으로 나눠줘야한다.
- 두 날짜간의 시간상의 전후를 알기위해서는 그냥 양수인지 음수인지를 파악하거나 after(Object when), before(Object when) 함수를 사용해도 된다.

```java
Calendar time1 = Calendar.getInstance();
Calendar time2 = Calendar.getInstance();
// long type으로 지정하는것이 좋다
long difference = Math.abs(time2.getTimeInMillis() - time1.getTimeInMillis()) / 1000;

System.out.println("time1과 time2의 차이는 " + difference + "입니다");

String tmp = "";
for(int i=0; i< TIME_UNIT.length; i++){
	tmp += difference/TIME_UNIT[i] + TIME_UNIT_NAME[i];
	difference %= TIME_UNIT[i];
}
System.out.println("시분초로 변환하면 " + tmp + "입니다.");
```
