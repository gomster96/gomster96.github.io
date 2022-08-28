---
title: SOLID를 적용하기 위한 해결책 IoC 그리고 DI
date: 2022-08-28 20:00:05
category: 'Spring'
draft: false
---

이전에 IoC와 DI 그리고 DL에 대해 간단하게 정리해본 적이 있다. 하지만 이번 시간에는 예제와 함께 어떤 이유에서 IoC, DI가 등장하였고, 스프링 컨테이너는 어떻게 이를 도와주는지를 알아보겠다.

# SOLID를 위반하는 코드

```java

public class MemberServiceImpl implements MemberService{

   private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}


```

만약에 위에 예시처럼 MemberService라는 interface를 구현한 MemberServiceImpl 이라는 것이 있을 때 우리는 memberRepository를 사용하기 위해 다음과 같이 사용한다. 하지만 이렇게 될 경우 문제가 발생한다.

- OCP (Open/Closed Principle) 개방 폐쇄 원칙의 위반
  - 만약에 MemoryMemberRepository를 DataBaseMemberRepository로 바꾸고 싶을 때, 위에코드에서 우리는 해당 부분을 다음과 같이 고칠 수 밖에 없다.

```java
    private final MemberRepository memberRepository = new MemoryMemberRepository();
    =>
    private final MemberRepository memberRepository = new DatabaseMemberRepository();
```

따라서 이는 Client 코드의 변경이므로 결국 OCP의 위반이다.

- DIP (Dependency Inversion Principle) 의존관계 역전 원칙의 위반
  - 프로그래머는 `추상화에 의존` 해야하지, `구현체에 의존`하면 안된다.
  - 하지만 위에 예시는 결국 MemberMemberRepository라는 구현체에 의존을 하게 되므로 DIP에 의존하게 된다.

# SOLID를 위반하지 않는 이상적인 코드

```java

public class MemberServiceImpl implements MemberService{

    private final MemberRepository memberRepository;

    // 생성자에 의해 주입을 받음
    // 따라서 memberRepository 라는 interface 만 가지고 있음 -> 추상화에만 의존
    // DIP 를 지키게 됨
    // 생성자 주입이라고 함
    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}


```

이상적인 코드라면 위에 예시처럼 MemberServiceImpl에서는 오직 추상화인 MemberRepository만 알고 있어야 하고, 이에 구현체에 관한 것은 어딘가에서 주입 받아야 한다.

-> 위에 예시처럼 되어 있는 것을 생성자 주입이라고 한다.

-> 이러한 방법으로 DIP, OCP를 위반하지 않을 수 있다.

# Config 파일의 필요성

하지만 위에 예시처럼 하려면, 어딘가에서는 구현체를 만들어주어야 한다. 따라서 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, `구현 객체를 생성하고, 연결하는 책임`을 가지는 별도의 설정 클래스를 만들 것이다.

- 이렇게 별도로 책임을 지는 설정클래스를 만듬으로써, MemberServiceImpl과 같은 클래스는 본인의 역할에만 충실하고, 이후 들어올 repository에 관한 구현체에는 상관하지 않은 채로, 구현체의 기능만 사용하면 된다.

## AppConfig

```java
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy(){
        //        return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

다음과 같은 방법을 사용하여, AppConfig 파일을 만든다면, 다른 파일에서는 단순히 아래와 같이 사용하면 된다.

```java
    MemberService memberService = AppConfig.memberService();
```

즉 구현체에 대한 것은 AppConfig 만 알면 되는것이고, 만약 구현체를 바꾸고 싶다면, 각 역할을 맡은 코드를 바꾸는 것이 아닌 AppConfig에 있는 구현체만 바꾸어주면 되는 방법이다.

이러한 방법은 `생성자를 통해서 인스턴스가 주입`되므로 생성자 주입이라고 한다.

- 따라서 이제부터는 MemberServiceImpl은 어떤 구현 객체(MemberRepository)가 들어올 지 모르게 된다. 의존관계에 대한 고민은 외부에 맡기고 행에만 집중하게 된다.

### DI(Dipendency Injection) 의존성 주입

이처럼 클라이언트인 memberServiceImpl의 입장에서는 의존관계를 마치 외부에서 주입해주는 것과 같다고 해서 이러한 것을 DI(Dipendency Injection) 의존성 주입이라고 한다.

# AppConfig 파일을 스프링컨테이너를 통해서 관리

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    @Bean
    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }
}
```

이전의 Config파일을 스프링 컨테이너에서 관리하는 방법이다.

크게 다른 건 없고, @Configuration이라는 어노테이션을 붙여준 뒤, 우리가 보내줄 구현체를 반환하는 메소드에는 @Bean 어노테이션을 붙여주면 된다.

또한 다른 클래스에서 해당 빈 즉 구현체를 사용하고 싶으면 다음과 같이 사용하면 된다.

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
```

ApplicationContext는 모든 빈들을 가지고 있는 Config 파일이라고 생각하면 되고, ApplicationContext의 getBean 메서드를 통해서 필요한 memberService, orderService 들을 받아올 수 있다.

# IoC, DI, 컨테이너

여기서는 위에서 아주 중요하게 다루었던 개념인 IoC, DI 그리고 컨테이너에 대해 다시 한번 정리할 것이다.

## 제어의 역전 IoC (Inversion of Control)

지금까지의 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고 실행했다. 구현객체가 프로그램의 제어 흐름을 스스로 조종했다.

하지만 AppConfig 가 등장한 이후부터, 구현객체는 자신의 로직을 실행하는 역할만 담당하고, 프로그램에 대한 제어 흐름에 대한 권한은 모두 AppConfig가 가지고 있다.

> 이렇게 프로그램의제어 흐름을 직접 제어하는 것이 아닌 외부에서 관리하는 것을 제어의 역전 (IoC) 라고 한다.

## 의존관계 주입 DI

의존 관계 주입을 사용하면 클라이언트의 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다. (외부에서 주입하는 방식으로, 클라이언트는 추상화만 가지고 있음)

> 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

## IoC 컨테이너, DI 컨테이너

AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해주는 것을 IoC컨테이너 또는 DI컨테이너라고한다.

# reference

- 김영한, Spring 기본편
