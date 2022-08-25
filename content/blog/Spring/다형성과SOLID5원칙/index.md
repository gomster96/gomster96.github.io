---
title: 다형성과 SOLID 5원칙
date: 2022-08-25 09:13:04
category: 'Spring'
draft: false
---

# 다형성이란?

```md
다형성이란 프로그램 언어 각 요소들(상수, 변수, 식, 객체, 메소드 등)이 다양한 자료 형(type)에 속하는 것이 허가되는 성질을 가르킨다.
```

즉 다형성이란 하나의 타입에 여러 객체를 대입할 수 있는 성질이라고 이해하면 된다. 따라서 다형성을 활용하면 기능을 확장하거나, 객체를 변경해야할 때 타입의 변경 없이 객체 주입만으로 수정이 일어나게 할 수 있다.

- 인터페이스를 구현한 객체 인스턴스를 실행 시점에 유연하게 변경할 수 있다.
- 클라이언트를 변경하지 않고, 서버의 구현 기능을 유연하게 변경할 수 있다.

# 역활과 구현을 분리

다형성을 효과적으로 활용하기 위해서, 역할과 구현을 확실하게 분리하는 것이 필요하다.

여기서 역할은 인터페이스이고, 구현은 해당 인터페이스를 구현한 클래스 또는 객체 인스턴스이다.

- 자동차를 예를 들면, 자동차의 기능 자체는 역할(interface)이 된다. 이후 자동차의 종류 아반떼, K5, 제네시스 등 자동차의 기능이라는 역할(interface)를 구현한 객체 들이 구현 부분이 된다.

# 다형성과 스프링

스프링은 다형성을 극대화해서 이용할 수 있게 도와준다. 스프링에서 제공하는 제어의 역전(IoC), 의존관계 주입(DI)는 다형성을 활용해서 역활과 구현을 편리하게 다룰 수 있도록 지원한다.

# 좋은 객체 지향 설계의 5가지 원칙(SOLID)

SOLID 5원칙은 클린코드로 유명한 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리한 것이다.

- SRP : 단일 책임 원칙 (Single Responsibility Principle)
- OCP : 개방-폐쇠 원칙 (Open/Closed Principle)
- LSP : 리스코프 치환 원칙 (Liskov substitution Principle)
- ISP : 인터페이스 분리 원칙 (Interface Segregation Principle)
- DIP : 의존관계 역전 원칙 (Dependency Inversion Principle)

## SRP 단일 책임 원칙 (Single Responsibility Principle)

- 한 클래스는 하나의 책임만 가져야 한다.
- 중요한 기준은 변경으로, 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것이다.

## OCP 개방-폐쇠 원칙 (Open/closed Principle)

- 소프트웨어 요소는 확장에는 열려 있으나, 변경에는 닫혀 있어야 한다.
- 다형성을 활용해보아야한다.

하지만 다음과 같은 예시를 보면, 분명 다형성을 이용하여 구현체를 바꿨지만, 클라이언트 코드를 변경하게 된다.

```java
public class  MemberService{

    // private MemberRepository memberRepository = new MemoryMemberRepository();
    private MemberRepository memberRepository = new JdbcMemberRepository();

}
```

따라서 이와 같은 문제점을 해결해주기 위해 나타난 것이, Spring Continaer이다.

## LSP 리스코프 치환 원칙

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야한다.
- 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다는 뜻이다. 다형성을 지원하기 위한 원칙으로, 인터페이스를 구녛나 구현체를 믿고 사용하려면 해당 원칙이 필요하다.

## ISP 인터페이스 분리 원칙

- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
- 예를 들어 자동차 인터페이스가 있다면 이를, 운전 인터페이스, 정비 인터페이스로 분리하는 것이 ISP의 예가 될 수 있다.

## DIP 의존 관계 역전

- 프로그래머는 "추상화에 의존해야지, 구체화에 의존하면 안된다."
- 역할(Role)에 의존하도록 코드를 구현해야한다.

위에서 사용했던 예시도 마찬가지로 DIP도 위반하는 예시이다.

```java
public class  MemberService{

    // private MemberRepository memberRepository = new MemoryMemberRepository();
    private MemberRepository memberRepository = new JdbcMemberRepository();

}
```

보면 memberRepository는 결국 jdbcMemberRepository라는 구현체에 의존하고 있으므로, DIP에 위반이다.
