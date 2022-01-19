---
title: Spring Controller의 return 타입
date: 2022-01-19 20:00:06
category: 'Spring'
draft: false
---

# Spring Controller 리턴 타입

Spring에서 지원해주는 Controller에는 다양한 리턴 타입을 지원하며 각각의 역할이 다르다. 이번 포스팅에서는 스프링에서 지원하는 리턴타입을 알아보도로 하겠다.

## String

Spring + View Template를 사용할 때 흔히 사용하는 타입이다. String에 View네임을 지정하면 지정한 뷰로 데이터가 전송되게 된다.

```java
@GetMapping("/test")
public String test(Model model) {
    model.addAttribute("data", data);
    return "/test/data";
}
```

이때 Model 안에 데이터를 key와 value값으로 담고, 이에 따른 return 타입을 String 값으로 뷰의 이름을 지정해주면 지정한 뷰로 이동하고, 이를 사용하는 템플릿의 문법에 맞게 사용하면 된다.

```jsp
...
    <body>
        <p>${data.name} => </p>
        <p${data.age} =></p>
    </body>
...
```

해당 예제는 JSP에서 값을 받을 떄 사용하는 방법이다. 이렇게 받고 사용하면 된다.

## ModelAndView

요즘은 잘 사용하진 않지만 View와 Object를 한번에 설정해주는 방법이다. new ModelAndView("페이지경로") 로 뷰를 정하고, addObject()메서드로 데이터를 저장한다.

```java
public ModelAndView test(){
    ModelAndView mav = new ModelAndView("test/viewPage");
    ModelAndView.addObject("data", "hihihi");
    return mav
    // 또는 아래처럼 체이닝으로 한번에 처리해줄 수도 있다.
    //return new ModelAndView("test/viewPage").addObject("data", "hihihi");
}
```

## redirect

돌아가려는 view의 `redirect:` 접두어를 붙이게 되는경우 지정한 페이지로 리다이렉트가 되게 된다. 상대경로, 절대경로 두가지 방식이 있다.

- redirect:/api/test -> 현재 서블릿 컨텍스트에 대한 상대적인 경로로 리다이렉트를 하게 된다.
- redirect:http//:localhost:8080/api/test -> 전체경로를 적는경우 절대 경로로 이동한다.

```java
public ModelAndView test() {
    ModelAndView mav = new ModelAndView();
    mav.setViewName("redirect:/api/test");
    return mav;
}
```

## void

Spring에서는 신기하게도? void 타입도 리턴이 가능하다. Spring은 뷰명을 입력하지 않아도 기본적으로 해당 url을 이용해서 뷰네임을 결정한다. 스프링 설정 파일에 실제 인터페이스는 RequestToViewNameTranslator 인데, 이 인터페이스의 빈이 존재하지 않을 경우, 기본적으로 DefaultRequestToViewNameTranslator구현체를 사용한다.

```java
@GetMapping("/test/void")
@public void test(Model model){
    model.setAttribute("data", data);
}
```

이렇게 할시에 요청 url의 제일앞 /와 확장자를 제외한 나머지 부분을 뷰네임으로 지정한다. 즉 여기서는 test/address가 뷰네임으로 지정된다.

## Model Object

여기서 Model은 Spring에서 제공하는 Model클래스가 아니라 우리가 자주 Model로서 사용하는 오브젝트 클래스를 이야기하는 것이다. 즉 Spring에서는 Model 그 자체를 리턴해도 좋다.

```java
public class Hello {
  private String name;
  private String email;

//...

@GetMapping("/hello")
public Hello hello() {
  return new Hello("gomster", "gomster96@gmail.com");
}
```

마찬가지로 void처럼 뷰네임을 설정할수 없기 때문에 RequestToViewNameTranslator 로 뷰네임을 지정해야한다.

## @ResponseBody

전통적인 스프링에서는 @Controller를 View를 반환하기 위해 사용했었지만, 요즘처럼 SPA로 개발을 하거나 Ajax로 개발을 할때 유용한 어노테이션이다. JSON형태로 데이터를 반환해 줄 수 있게 한다. REST SERVER로서의 역할을 할 떄 사용한다.

```java
@Controller
public class TestController {

	@GetMapping("/test/account")
	@ResponseBody
	public Account void(Account Account) {
  		return account;
	}
}
```

# reference

- https://ooeunz.tistory.com/101
