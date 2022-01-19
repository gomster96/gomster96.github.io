---
title: Spring Controller의 parameter를 받아오는 방법
date: 2022-01-19 22:31:00
category: 'Spring'
draft: true
---

# 예제는 내가 바꾸기

# Reqequest를 통해 파라미터를가져오는 방법

## httpServletRequest.getParameter()

```java
@RequestMapping("/test")
    String test(HttpServletRequest request)
    {
        String a = request.getParameter("a");
        String b = request.getParameter("b");

        System.out.println("a: " + a);
        System.out.println("b: " + b);
    }
```

## Map사용

```
@RequestMapping("/map")
	String temp2(@RequestParam Map<String, String> param)
	{
		String a = param.get("a");
		String b = param.get("b");

		System.out.println("a : " + a);
		System.out.println("b : " + b);

		return "data";
	}
```

@ RequestParam을 통한 직접 매칭

```java
@RequestMapping( "/param")
	String temp3(@RequestParam("a") String a, @RequestParam("b") int b)
	{
		System.out.println("a : " + a);
		// Integer.parseInt() 과정이 필요없다!
		System.out.println("b : " + b);

		return "data";
	}
```

## 모델 클래스를 통한 직접 매칭

```java
@RequestMapping("/class")
	String temp4(userVO abc)
	{
		System.out.println("a : " + abc.getA());
		System.out.println("b : " + abc.getB());

		return "data";
	}
```

userVO.java

```java
package com.study.parameter.VO;

public class userVO {
	private String a;
	private int b;

	public String getA() {
		return a;
	}
	public void setA(String a) {
		this.a = a;
	}
	public int getB() {
		return b;
	}
	public void setB(int b) {
		this.b = b;
	}

	@Override
    public String toString(){
        return "userVO [a = " + a + ", b = " + b + "]";
    }

}
```

# reference

- https://velog.io/@ye050425/spring-controller-parameter-%EC%A0%95%EB%A6%AC
