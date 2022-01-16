---
title: Maven 이란
date: 2022-01-16 15:06:04
category: 'Spring'
draft: true
---

# Maven 메이븐이란?

Apache Maven의 정의는 다음과 같다.

```
Apache Maven은 자바용 프로젝트 관리도구로 Apache Ant의 대안으로 만들어졌다. Apache License로 배포되는 오픈 소스 소프트웨어이다.
```

위의 정의로는 잘 이해가 되지 않으므로 조금 더 블로그를 찾아보고 알게된 부연설명을 하자면

- Maven은 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면 네트워크를 통해서 내가 사용할, 필요한 라이브러리들을 관리하여 자동으로 다운받아준다.
- Maven은 중앙 저장소를 통한 자동 의존성 관리를 하고 이때 중앙 저장소(아파치 재단에서 운영 관리함)는 라이브러리를 공유하는 파일 서버라고 볼 수 있다.

# Maven Lifcycle

메이븐은 프레임워크이기 때문에 동작 방식이 정해져 있고, 메이븐에서 미리 정의되어 있는 Build들의 순서를 라이프사이클(Lifecycle)이라고 한다.

- 미리 정의된 빌드순서를 라이프사이클(Lifecycle)이라 하고, 각 빌드 단계를 Phase라고 한다.

## Life사이클의 순서

- Default(Build) : 일반적인 빌드 프로세스를 위한 모델이다.
- Clean : 빌드 시 생성되었던 파일들을 삭제하는 단계
- Validate : 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인하는 단계
- Compile : 프로젝트의 소스코드를 컴파일 하는 단계
- Test : 유닛(단위) 테스트를 수행 하는 단계(테스트 실패시 빌드 실패로 처리, 스킵 가능)
- Pacakge : 실제 컴파일된 소스 코드와 리소스들을 jar, war 등등의 파일 등의 배포를 위한 패키지로 만드는 단계
- Verify : 통합 테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계
- Install : 패키지를 로컬 저장소에 설치하는 단계
- Site : 프로젝트 문서와 사이트 작성, 생성하는 단계
- Deploy : 만들어진 package를 원격 저장소에 release 하는 단계

# maven 파일

1. web.xml

- 최초로 WAS가 구동될 떄 각종 설정을 정의해준다.
- 여러 xml파일을 인식하도록 각 파일을 가리켜준다.

# maven 사용

pom.xml에 사용할 라이브러리를 검색해서 추가해준다.
[메이븐 라이브러리](https://mvnrepository.com/artifact/org.mybatis/mybatis-spring/2.0.6)

# Reference

- https://goddaehee.tistory.com/199 [갓대희의 작은공간]
