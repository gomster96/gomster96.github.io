---
title: WebClient
date: 2022-09-11 15:06:05
category: 'Spring'
draft: false
---

<!-- <p align="center"><img src="1.png" height="250px" width="600px"></p> -->

# WebClient란?

WebClient는 Spring에서 제공하는 RestClient의 한 종류이다. RestTemplate는 점진적으로 Deprecated 될 예정이기 때문에 이제는 WebClient를 사용하는 것이 좋다.

## RestTemplate와 WebClient

### RestTemplate란?

RestTemplate은 스프링에서 제공하는 템플릿이다. 스프링 3.0에서부터 지원하는 RestTemplate은 HTTP 통신에 유용하게 쓸 수 있다. REST 서비스를 호출하도록 설계되어 HTTP 프로토콜의 메서드 (GET, POST, DELETE, PUT)에 맞게 여러 메서드를 제공한다. RestTemplate은 다음과 같은 특징이 있다

- 통신을 단순화하고 RESTful 원칙을 지킨다
- 멀티쓰레드 방식을 사용
- Blocking 방식을 사용

### WebClient란?

WebCleint는 스프링 5에서 추가된 인터페이스다. 스프링 5 이전에는 비동기 클라이언트로 AsyncRestTemplate을 사용했었다. 하지만 스프링 5 이후 부터는 WebClient를 사용하는 것을 권장한다. WebClient는 다음과 같은 특징이 있다.

- 싱글 스레드 방식을 사용
- Non-Blocking 방식을 사용
- JSON, XML을 쉽게 응답받는다.

## WebClinet 생성법

1. WebClient.create();

```java
WebClient webClient = WebClient.create();
```

2. Builder를 활용한 생성

```java
WebClient webClient = WebClient.builder()
                                .baseUrl("http://localhost:8080")
                                .defaultCookie("쿠키","쿠키값")
                                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
	                            .build();
```

## WebClient로 Reqeust 보내기

### webClient.create()webClient를 생성 했을 경우

```java
webClient.get()
         .uri("요청 보낼 URI입력")
         .headers(header ->{
            header.setContentType(<ContenstType>)
            header.setAcceptCharset(<CharsetType>)
         }) // 필요시 header 밑 관련 조건 추가 가능
        .retrieve()
```

```java
webClient.post()
         .uri("요청 보낼 URI입력")
         .headers(header ->{
            header.setContentType(<ContenstType>)
            header.setAcceptCharset(<CharsetType>)
         }) // 필요시 header 밑 관련 조건 추가 가능
         .bodyValue(<formdata가 MultiValueMap<String,String>type으로 들어감>)
        .retrieve()
```

### Builder로 webClient를 생성했을 경우

```java
WebClient.RequestHeaderUriSpec<?> baseSpec = WebClient.builder()
                                                      .baseUrl("http://localhost:8080")
	                                                    .build()
                                                      .get();


// 이후 baseSpec에 원하는 파라미터를 추가로 붙여서 요청한다.
baseSpec.uri(builder -> builder.path("/")
                               .queryParam("이름","값")
                               .builder()
                        )
                        .retrieve() //Response를 받아옴.

```

## WebClient로 response받아오기

- exchange()
  ClientResponse를 상태값, 그리고 헤더와 함께 가져온다.
- retrieve()
  body를 바로 가져온다.

```java
String response = req.exchange().block().bodyToMono(String.class).block();
String response = req2.retrieve().bodyToMono(String.class).block();
```

block을 사용하면 RestTemplate처럼 동기식으로 사용할 수 있다. block이후의 반환 값은 String으로 받을 수도 있지만, `Map<String, Object>`꼴로 더 편하게 받을 수 있다.

bodyToMono는 가져온 body를 Reactor의 Mono객체로 바꿔준다.

- Mono객체는 0~1개의 결과를 처리하는 객체이다.
- Flux는 0~N개의 결과를 처리하는 객체이다.

[Mono, Flux, 그리고 WebFlux에 대한 설명](https://pearlluck.tistory.com/730)

# Reference

- https://pearlluck.tistory.com/730 : WebFlux에 대해 익히기
- https://www.notion.so/RestTemplate-WebClient-1f7c27dcb3ff4f4b8bc4e160c3666a61
- https://weirdstuffs.tistory.com/3
