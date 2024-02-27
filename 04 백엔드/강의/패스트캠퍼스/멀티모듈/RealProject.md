# 비동기
참고 : 
[logicbig.com/tutorials/core-java-tutorial/java-multi-threading/thread-pools.html](https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/thread-pools.html)
https://www.baeldung.com/thread-pool-java-and-guava

## 비동기에 쓰레드 풀을 사용하는 이유는?
![[Pasted image 20230823054603.png]]
쓰레드도 하나의 자원이므로 **계속적으로 소모되면 자원고갈**로 인해 메모리풀로 인해 서버가 다운될 수 있다.

쓰레드를 미리 만들어 놓고 재사용하는 방식인 쓰레드 풀을 사용함.


## 코드로 어떤걸 구현해야 할까?
![[KakaoTalk_20230823_055129834.jpg]]
코드 : [realProjectPractice/Async](https://github.com/sprae114/realProjectPractice/tree/master/Async)


# Feign Client
공식문서 : [Spring Cloud OpenFeign](https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/)
## Feign Client란?
Netflix에서 개발한 **선언적(Declarative) 웹 서비스 클라이언트**입니다. 스프링 클라우드 기반의 마이크로서비스 아키텍처에서 사용되며, **다른 마이크로서비스에 쉽게 접근하고 호출**할 수 있도록 돕는 도구입니다.

인터페이스와 에노테이션을 사용하여 다른 마이크로서비스의 RESTful API를 호출할 수 있는 프록시를 제공합니다.
![[KakaoTalk_20230823_072037400.jpg]]

## 코드로 어떤걸 구현해야 할까?
참조 : [realProjectPractice/feign](https://github.com/sprae114/realProjectPractice/tree/master/feign)

![[KakaoTalk_20230823_072037400_01.jpg]]


## 클래스 및 설정 설명
#### Connection/Read Timeout
**연결 및 읽기 시간 초과를 설정**할 수 있습니다. 이를 통해 마이크로서비스 간의 통신에서 일정 시간동안 응답이 없을 때 자동으로 연결을 종료할 수 있습니다

#### Feign Interceptor
**외부로 요청이 나가기전에 공통적으로 처리**가 가능함. 이를 통해 요청 헤더 수정, 인증 정보 추가, 요청 로깅 등 다양한 기능을 구현할 수 있습니다.


#### Feign Logger
Feign 요청과 응답에 대한 로깅을 제공합니다. 요청 및 응답을 추적하거나 디버깅하는데 유용하게 사용할 수 있습니다.


#### Feign ErrorDecoder
원격 서비스 호출에서 에러가 발생했을 때, **에러 응답을 커스텀한 예외로 처리**할 수 있습니다. 이를 통해 일관된 예외 처리 및 에러 처리 로직을 구현할 수 있습니다.


# logback
