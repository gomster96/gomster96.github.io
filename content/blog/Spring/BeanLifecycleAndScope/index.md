---
title: 스프링 빈 생명주기, 콜백, 스코프
date: 2022-09-05 09:13:04
category: 'Spring'
draft: false
---

# 스프링 빈이란?

`Spring IoC 컨테이너가 관리하는 자바 객체`를 빈(bean)이라고 한다. IoC(제어의 역전)에서 다루었지만 우리가 직접 new를 통해 생성하는 객체가 아닌, `Spring IoC 컨테이너`에 의하여 생성, 관리 되는 자바 객체이다.

# 스프링 빈의 라이프사이클

스프링 빈은 간단하게 다음과 같은 라이프 사이클을 가진다.

> 객체 생성 -> 의존관계 주입

따라서 스프링 빈은 객체를 생성하고, 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다. 하지만 `개발자가 의존관계 주입이 완료 된 시점, 스프링 빈이 소멸되기 직전 시점` 등을 알기 위해 해당 시점을 알게해주는 기능을 `콜백 메서드`를 통해 제공한다.

### 스프링 빈의 이벤트 라이프 사이클

```
1. 스프링 컨테이너 생성
2. 스프링 빈 생성
3. 의존관계 주입
4. 초기화 콜백 : 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
5. 사용
6. 소멸전 콜백 : 빈이 소멸되기 직전에 호출
7. 스프링 종료
```

## 스프링이 빈 생명주기 콜백을 지원하는 방법

3가지 방법을 소개할 것이지만, 결국 맨 마지막에 소개하는 `@PostConstoructor, @PreDestory` 방법을 사용하면 된다.

### 1. 인터페이스(InitializingBean, DisposableBean)

InitializingBean, DisposableBean 두 interface를 implements하면 afterPropertiesSet(), destory() 메서드를 오버라이드해서 사용할 수 있게 된다.

### 2. 설정 정보 초기화 메서드, 종료 메서드 지정

```java
@Configguration
static class LifeCycleConfig{
  @Bean(initMethod = "초기화콜백메서드", destoryMethod = "소멸전콜백메서드")
  public MyBeanClass myBeanClass(){ }
}

```

### 3. @PostConstructor, @PreDestory

사용하고 싶은 초기화 콜백 메서드(@PostConstructor)와 소멸 전 콜백 메서드(@PreDestory) 위에 위에 어노테이션을 붙여주면 콜백메서드를 사용할 수 있다.

# 빈스코프

빈 스코프란 빈이 존재할 수 있는 범위를 뜻한다. 기본적으로 스프링 빈은 싱글톤 스코프로 생성되기 때문에 스프링 컨테이너의 시작과 함께 생성되어서 스프링 컨테이너가 종료될 때까지 유지된다.

- 싱글톤 : 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다.
- 프로토타입 : 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고, 더는 관리하지 않는 매우 짧은 범위의 스코프이다.
- 웹 관련 스코프
  - request : Http 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리된다.
  - session : HTTP Session과 동일한 생명주기를 가지는 스코프
  - application : 서블릿 컨텍스트(ServletContext)와 동일한 생명주기를 가지는 스코프
  - websocket : 웹 소켓과 동일한 생명주기를 가지는 스코프

# reference

- 김영한, Spring 기본편
