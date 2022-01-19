---
title: IoC, DI, DL
date: 2022-01-19 20:00:05
category: 'Spring'
draft: false
---

# IoC(Inversion of Control)

IoC는 Inversion of Control의 약자로서 제어권의 역전이라는 뜻이다.
`객체의 생성부터 생명주기의 관리까지 객체에 대한 제어권을 개발자에서 스프링 컨테이너가 담당하도록 역전` 된 것을 의미한다. 따라서 객체의 생성과 소멸, 객체간의 의존 관계를 개발자가 아닌 Spring 프레임워크가 통제하고 관리하게 된 것이다. 이렇게 IoC개념을 구현하기 위해서 DI와 DL이 나왔다.

## 종속성(Dependency)

아래의 예제를 봐보면

```java
class Computer{
    private OS os;
    private String name;
    private long money;

    public void setOS(OS os){
        this.os = os;
    }
}
class OS(){
    private String osName;
    private double version;
}
```

이렇게 Computer 객체는 반드시 OS객체를 가져야만 한다. 즉 OS는 Computer 클래스를 위한 반드시 필요한 클래스가 된다. 이러한 것을 종속성(의존성)을 가진다라고 할 수 있다.

## 스프링 빈(Bean)

자바 빈이랑 의미가 다른데 스프링 빈은 Spring IOC 컨테이너가 관리하는 자바객체를 빈(Bean)이라는 용어로 부른다.

# DL(Dependency Lookup)

DL은 의존성 검색이다. 이는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup 즉 찾는 것이다.

아래와 같이 Bean에 대한 정보가 있는 xml파일이 있다면

myWeb.xml

```xml
<beans>
    <bean id="myObject" class="com.example.MyObject"/>
</beans>
```

java에서는 이를 보고 어떤 클래스를 사용할지 검색하여 주입하게 된다.

```java
String myConfigLocation = "classpath:myWeb.xml"
AbstractApplicationContext ctx = new GenericXmlApplicationContext(myConfigLocation);
MyObject myObject = ctx.getBean("myObject", MyObject.class);
```

이러한 방법으로 적절한 MyObject클래스를 가지고 올 수 있게된다.

# DI(Dependency Injection)

DI는 Dependency Injection의 약자로서 종속성 주입이라는 뜻이다. 말 그대로 의존관계를 주입 시키는 기능으로, 객체를 직접 생성하는 것이 아니라 외부에서 생성한 후 이를 주입해주는 것을 의미한다.

-> DI를 통해 외부에서 객체를 생성하여 주입함으로 객체끼리의 불필요한 의존성을 없애거나 줄일 수 있게 된다.

### 객체 A와 관련있는 객체 a,b가 있을 때

- 기존 방법 : 서로 관련있는 객체는 객체 내부에서 new 메서드를 통해 객체를 생성해 주었다.
- DI : 외부에서 생성된 객체를 setter()나 생성자를 통해 의존성을 받을 수 있다.

# DI의 세가지 방법

의존성 주입에는 3가지 방법이 있다.

1. Constructor Injection (생성자 주입)
2. Field Injection (필드 주입)
3. Setter Injection(세터 주입)

[버터필드님의 블로그](https://atoz-develop.tistory.com/entry/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI-Dependency-Injection%EC%9D%98-%EC%84%B8%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95?category=869240)의 예제가 내용이 좋아가지고 왔다.

## 1. 생성자 주입

```java
@Component
public class SampleController {
    private SampleRepository sampleRepository;

    @Autowired
    public SampleController(SampleRepository sampleRepository) {
        this.sampleRepository = sampleRepository;
    }
}

```

이렇게 생성자에 @Autowired 어노테이션을 붙이면 SampleRepository를 즉 의존성을 주입받을 수 있다.

- Spring 4.3부터 클래스의 생성자가 하닝고, 그 생성자로 주입받을 객체가 빈으로 등록되어 있다면 생성자 주입에서 @Autowired를 생략 할 수 있다.

## 2. 필드 주입

```java
@Component
public class SampleController {
    @Autowired
    private SampleRepository sampleRepository;
}
```

인스턴스 변수 선언 위에 @Autowired 애너테이션을 붙임으로서 의존성을 주입받을 수 있다.

## 3. Setter 주입

```java
@Component
public class SampleController {
    private SampleRepository sampleRepository;

    @Autowired
    public void setSampleRepository(SampleRepository sampleRepository) {
        this.sampleRepository = sampleRepository;
    }
}

```

setter 메소드 위에 @Autowired 어노테이션을 붙여 만들 수 있다

3 방법 다 사용할 수 있지만 Springframework reference에서 권장하는 방법은 생성자를 통한 DI이다.

- 의존성 없이는 인스턴스를 만들지 못하도록 강제할 수 있기 때문이다.

# reference

- https://atoz-develop.tistory.com/entry/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI-Dependency-Injection%EC%9D%98-%EC%84%B8%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95?category=869240
- https://juyoungit.tistory.com/122?category=871743
- https://velog.io/@ggomjae/Spring-Framework-DI-Dependency-Injection
- https://velog.io/@bahar-j/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85Dependency-Injection
- https://happy-playboy.tistory.com/entry/%EB%B0%B1%EC%88%98%EC%9D%98-%EC%8A%A4%ED%94%84%EB%A7%81-IoC%EC%99%80-DI%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C
