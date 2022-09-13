---
title: Argument Resolver
date: 2022-09-13 15:06:05
category: 'Spring'
draft: false
---

<!-- <p align="center"><img src="1.png" height="250px" width="600px"></p> -->

# Arguement Resolver란

Arguemnt Resolver는 Spring 환경에서 Controller로 들어온 파라미터를 가공하거나, 수정, 바인딩 기능을 제공할 때 사용하는 객체이다. `HandlerMethodArgumentResolver` 인터페이스를 상속하여 Class를 만들어 사용한다. Body(@Request Body)에 담겨 들어오거나 @PathVariable을 이용하는 데이터들은 Controller에서 바로 파라미터로 받을 수 있지만 `세션, 쿠키, 헤더` 등에서 제공받는 데이터들을 파라미터로 받는 경우 Argument Resolver로 바인딩 할 수 있다.

Argument Resolver를 사용하면, 반복코드를 확실히 줄여줄 수 있다.

# 사용 예시

ArgumentResolver를 Spring프레임워크 내에서 사용할 수 있도록 하는 방법은 다음과 같다.

1. `HandlerMethodArgumentResolver` 인터페이스를 implements하는 ArgumentResolver 클래스를 만든다.
2. Config파일에서 `addArgumentResolvers` 메서도를 통해 resolvers에 add해준다.

## 1. HandlerMethodArgumentResolver를 사용한 클래스 생성

```java
@Component
@RequiredArgsConstructor
public class LoginMemberArgumentResolver implements HandlerMethodArgumentResolver {
    private final AuthService authService;

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.hasParameterAnnotation(Login.class);
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        String accessToken = AuthorizationExtractor
                .extractAccessToken(Objects.requireNonNull(webRequest.getNativeRequest(HttpServletRequest.class)));
        return authService.findMemberByToken(accessToken);
    }
}

```

나는 다음과 같이 사용하였다.

### suportsParameter(MethodParameter parameter)

parameter를 보고, 아래 resolverArgument의 동작여부가 결정난다.

- 리턴 타입이 true이면 resolveArgument메서드가 작동한다.
- 리턴 타입이 false이면 resolveArgument메서드가 작동하지 않는다.

```java
@Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.hasParameterAnnotation(Login.class);
    }
```

위의 코드는 해당 메서드가 Login.class라는 어노테이션을 가지고 있으면 resolveArgument메서드가 동작하도록 하는 것이다.

### resolveArgument

resolveArgument method 는 다음과 같다.

```java
public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                              NativeWebRequest webReqeust, WebDataBinderFactory binderFactory)
```

메소드는 nativeWebRequest 파라미터를 통해 HttpServletRequest형의 request를 얻게 되고, 이것은 실제로 호출된 Controller의 메소드로 향하던 request이다.

resolveArgument 메소드는 원리 러턴형이 Object이기에 원하는 타입으로 설계를 할 수가 있다.

```java
@Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        String accessToken = AuthorizationExtractor
                .extractAccessToken(Objects.requireNonNull(webRequest.getNativeRequest(HttpServletRequest.class)));
        return authService.findMemberByToken(accessToken);
    }
```

위에 예시코드는 로그인 여부를 확인하는 비즈니스 로직이 들어가 있는 resolveArgument이다.

위에예시처럼 nativeWebRequest형의 webRequest를 통해 토큰을 추출한다.

### Config파일에 add

```java
@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {
    private final LoginMemberArgumentResolver loginMemberArgumentResolver;


    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginMemberArgumentResolver);
    }
}
```

다음과 같이 WebMvcConfigurer을 implements한 WebConfig에 addArgumentResolvers를 override한다.

resolvers에 위에서 만든 loginMemberArgumentResolver를 add해줌으로서 resolvers에 더해준다.

이제 아래와 같이 @Login을 사용함으로써 ArgumentResolver를 호출할 수 있다.

```java

@GetMapping
    @RequiredLogin
    public ResponseEntity<Long> testLoginInterceptor(@Login LoginMember loginMember){
        return ResponseEntity.ok(loginMember.getId());
    }
```

이렇게 parameter로 `@Login LoginMember loginmember`를 호출하면 ArgumentResolver의 로직에 따라 결국 LoginMember꼴의 객체를 반환해준다.

# Reference

- https://minkwon4.tistory.com/264
