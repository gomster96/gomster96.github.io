---
title: DAO, DTO, VO 란?
date: 2022-01-18 07:40:04
category: 'Spring'
draft: false
---

# DAO (Data Accesss Object)

DAO는 Database에 접근하기위한 객체이며 Database 접근을 하기 위한 로직과 비즈니스 로직을 분리하기 위해 사용한다.

- Interface를 만들면 DAO는 이 Interface를 구현한 객체를 사용할 수 있도록 반환해준다.
- DAO는 DB연결를 해주는 Connection까지 설정 해줄 때가 많다. 단 MyBatis 등을 사용할 경우 커넥션 풀까지 제공하기 때문에 DAO를 별도로 만들지 않는다.

# DTO(Data Transfer Object)

DTO는 계층 간 데이터 교환을 하기위한 자바 빈즈(Java Beans)이다. 자바 빈즈이기 때문에 DTO는 로직을 가지지 않는, 데이터베이스 레코드의 데이터를 매핑하기 위한 순수한 데이터 객체(getter와 setter만 가짐)이다.

- DTO는 Database에서 Data를 얻어 Service나 Controller등으로 보낼 떄 사용하는 객체이다.
- 유저가 form으로 데이터를 넣을 때도 사용이 가능하다.
  - 유저가 브라우저에서 데이터를 입력하여 form에 있는 데이터를 DTO에 넣어서 전송한다. -> 해당 DTO를 받은 서버가 DAO를 이용하여 데이터베이스로 데이터를 집어 넣는다.

# VO(Value Object)

VO는 DTO와 혼용해서 사용하지만 오직 값 오브젝트로서 값을 위해 쓰인다. 즉 read-only 라는 특징을 가지기 때문에 사용하는 도중 값 변경이 불가능하며 오직 읽기만 가능하다.

- setter없이 getter만 사용한다.

# reference

- http://melonicedlatte.com/2021/07/24/231500.html
- https://m.blog.naver.com/cjhol2107/221757079506
