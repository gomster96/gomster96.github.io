---
title: '값 타입'
date: 2022-03-03 18:26:08
category: 'JPA'
draft: false
---

# JPA의 데이터 타입

## 엔티티 타입

- @Entity로 정의하는 객체
- 데이터가 변해도 식별자(ex: ID) 같은 것으로 지속해서 추적 가능
  - ex) 회원 엔티티의 키나 나이 값을 변경해도 식별자로 어떤 데이터인지 인식 가능

## 값 타입

- int, Integer, String처럼 단순히 값으로 사용하는 자바 기본 타입이나 객체
- 식별자가 없고 값만 있으므로 변경시 추적 불가
  - ex) 숫자 100을 200으로 변경하면 완전히 다른 값으로 대체

### 기본값 타입

- 자바 기본 타입(int, double 등)
- 래퍼 클래스(Integer, Long 등)
- 생명 주기를 엔티티의 의존
  - 엔티티가 삭제되면 기본값 타입의 필드도 알아서 삭제 됨
- 값 타입은 공유하면 안됌
  - ex) 회원이라는 테이블이 있을 때 회원의 이름을 변경시 다른 회원의 이름도 함께 변경되는 이런 일이 발생하면 안됌

### 임베디드 타입

- 새로운 값 타입을 `직접 정의`하는 것
- JPA는 임베디드 타입(embedded type) 이라고 하지만, 주로 기본 값 타입을 모아 만들기에 복합 값 타입 이라고도 함
- 임베디드 타입은 int, String 처럼 값 타입이다.

예시 - 어떤 레코드의 생성, 수정, 내용을 값 타입으로 저장하고 싶으면?

```java
@Embeddable // 값 타입을 정의하는 곳에 표시한다.
public class Info{
  private String content;
  private LocalDateTime createdAt;
  private LocalDateTime revisedAt;

  public Info(){
    // 기본생성자는 필수로 필요하다.
  }
  public Info(String content){
    this.content = content;
    this.createdAt = this.revisedAt = new Date();
  }

  public LocalDateTime getExistTime(){
      // createdAt과 현재시간을 이용해서 계산하여 리턴
  }
}
```

- @Embeddable은 값 타입을 정의하는 곳에서 표시한다.
- @Embedded는 값 타입을 사용하는 곳에 표시한다.
- 임베디드 타입은 재사용에 용의하다.
- 기본생성자는 반드시 필요하다.
- getExistTime 처럼 해당 값 타입에서만 사용하는 의미있는 메소드를 만들 수 있다.
- 임베디드 타입을 포함한 모든 값타입의 생명주기는, 일반 값 타입처럼 해당 값을 소유한 엔티티에 생명주기에 의존한다.

# Reference

- [김영한, 자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
