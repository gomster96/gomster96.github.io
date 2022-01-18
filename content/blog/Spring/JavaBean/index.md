---
title: Java Bean
date: 2022-01-18 07:41:04
category: 'Spring'
draft: false
---

Spring을 처음 공부해오면서, JSP를 공부하면서 자주 나오는 이름이 있었다. 그것은 바로 Java Bean 자바빈(자바빈즈)이다. 대체 빈이 무엇인지, 무슨 역할을 하는지 이번 포스팅에서 정리해보도록 하겠다.

# Java Bean이란

자바빈(Java Bean)는 자바로 작성된 소프트웨어 컴포넌트를 일컫는 말로 데이터 표현을 목적으로 하는 자바 클래스이며 다음의 표준 규약이 있다.

- 클래스는 인자(Argument)가 없는 기본 생성자(Default constructor)를 갖는다.
- 클래스의 멤버 변수는 프로퍼티(Properties)라고 하며 private 접근 제한자를 가져야 한다.
- 클래스의 프로퍼티들은 Getter/Setter를 통해 접근할 수 있어야 한다
  - Getter의 이름은 get{Properties_name} 이며, Setter의 이름은 set{Properties_name}이다
  - Getter/setter의 접근 제한자는 public이어야 한다.
  - 프로퍼티의 타입이 Boolean인 경우 is로 시작할 수 있다
  - Getter의 경우 파라미터가 존재하지않아야 하며, setter의 경우 하나 이상의 파라미터가 존재한다
- Read Only인 경우 Setter는 없을 수 있다.
- Serializable 인터페이스를 구현한다.
- 자바빈 클래스는 패키징 되어야 한다. (Package)

다음은 위키에나와있는 자바빈의 예제이다.

```java
public class PersonBean implements java.io.Serializable
{
    private String name;
    private boolean coding;

    // 기본 생성자 (인자가 없는).
    public PersonBean()
    {

    }

    public String getName()
    {
        return this.name;
    }
    public void setName(String name)
    {
        this.name = name;
    }

    // Different semantics for a boolean field (is vs. get)

    public boolean isCoding()
    {
        return this.coding;
    }

    public void setCoding(boolean coding)
    {
        this.coding = coding;
    }
}
```

보면 위의 규약들을 지킨 클래스라고 말할 수 있다. 즉 위에서 말한 자바빈의 규약들을 지킨 자바 클래스를 자바빈라고 한다.

### 자바빈은 무엇을 위한 규칙인가?

JavaBean의 목적은 여러가지 다른 오브젝트 들을 하나의 오브젝트(Bean)에 담기 위함이다.

### java.io.Serializable 인터페이스 구현 이유

File I/O, 네트워크 통신등을 할때 Stream으로 변환을 해주어야하는데, 객체를 Stream으로 변환해주는 과정을 직렬화(Serilization)이라고 한다. 자바빈의 목적은 여러가지 오브젝트들을 하나의 오브젝트에 담기 위해서 이고, 이를 담아서 File IO나 DB에 저장을 해준다. 이때 Stream으로 변환을 하기위해 Serializable인터페이스가 포함된다.

### vsSpring Bean

Spring에서도 Bean이라는 개념이 나와 많이 햇갈렸는데 Spring에서 Bean이란 **스프링 IoC컨테이너가 관리하는 Java 객체**를 뜻한다.

스프링 IoC가 관리하는 객체는 **스프링에 의해 생성**되고, **라이프 사이클을 수행**하고, **의존성 주입**이 일어나는 객체를 말한다.

- 개발자가 관리하는 객체가 아닌 스프링에게 제어권을 넘긴 객체를 스프링에서 Bean이라고 한다.

## 자바빈과 JSP

자바빈을 이용하면 JSP페이지를 보다 간단하게 표현해줄 수 있다. form에 있는 데이터들을 한번에 받아 자바빈즈 객체에 저장할 수 있고 생성할 수도 있다.

관련 예제들은 다른분들이 만들어 주신 예제를 참고하자.

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=rjs5730&logNo=220989038425
- https://pjh3749.tistory.com/75

# reference

- https://soft.plusblog.co.kr/58
- https://velog.io/@dion/what-is-javabeans-and-why-use-javabeans
- https://jjingho.tistory.com/10
- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=rjs5730&logNo=220989038425
