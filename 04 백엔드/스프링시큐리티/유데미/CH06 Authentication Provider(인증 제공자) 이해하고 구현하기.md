# 요약
[퀴즈](https://www.udemy.com/course/spring-security-6-jwt-oauth2-korean/learn/quiz/6534501#overview)
- `AuthenticationProvider`에 대해서 알 수 있음(p.45~46)
- `CustomAuthenticationProvider`를 구현할 수 있음(p.47)
- 프로파일을 이용해 운영환경과 개발환경을 구별할 수 있음(p.48~50)
![[Pasted image 20230829203309.png]]

# 45. 자신만의 AuthenticationProvider를 만드는 것을 고려해야 하는 이유
- 인증 제공자에 대해 동일한 프로세스로 모든 내용을 다루겠습니다. 
- 자체 인증 제공자를 정의하는 방법도 설명하겠습니다.

## DaoAuthenticationProvider이란? ✔
- Spring Security에서 사용자 인증을 처리하는 데 사용되는 클래스입니다. 주로 **데이터베이스에 저장된 사용자 정보를 기반으로 인증을 수행**합니다.
![[Pasted image 20240925202808.png]]


## 왜 AuthenticationProvider에 대해 배워야 할까요? ✔
 하지만 특정 시나리오에서는 사용자 정의 인증 제공자를 정의해야 할 필요가 있습니다. 실제 프로젝트에서는 보안 요구 사항이 간단하지 않을 수 있습니다. 
 
 1)  **다양한 로그인 방식을 지원하는 경우**
- 최종 사용자가 사용자 이름과 비밀번호로 로그인
- OAUTH2 프레임워크를 통한 인증
- JAAS를 통한 인증 요구(Java 인증 및 권한 부여 서비스로, Java 라이브러리 내의 보안 플러그)

![[Spring_Security_46.png]]


2) **특정 사용자 정의 요구 사항이 있을 경우**
기본 인증 제공자인 **`DaoAuthenticationProvider`는 일반적인 사용자 세부 정보 로드에 도움**을 줍니다. 특정 사용자 정의 요구 사항이 있을 경우, 자체 `AuthenticationProvider` 구현이 필요합니다.
- 사용자의 나이가 18세 이상일 때만 인증
- 특정 국가에서만 인증 허용
![[Pasted image 20230827204157.png]]


# 46. AuthenticationProvider 메소드 이해하기
이번 섹션에서는 인증 제공자에 대해 더 자세히 알아보겠습니다.

## AuthenticationProvider 인터페이스
Spring Security에서 **사용자의 인증을 처리하는 데 필요한 메서드를 정의**하는 인터페이스입니다.
![[Spring_Security_47.png]]

▶ `authenticate()` 메소드
- 이 메서드는 **인증 객체(Token)를 받아서 인증을 수행하고, 인증 객체(Authentication)를 반환**합니다. 이 메서드 내에서 **모든 사용자 정의 인증 로직을 구현할 수** 있습니다. 
- `AbstractUserDetailsAuthenticationProvider` 안에 `authenticate()` 메소드 살펴보기

▶ `supports()` 메소드
- 이 메서드는 현재 인증 제공자가 제공된 **인증 객체의 타입을 지원하는지 여부를 확인**합니다. 이 메서드는 true를 반환하도록 구현해야 합니다.
- `DaoAuthenticationProvider` 경우 `supports()` 메소드는 `UsernamePasswordAuthenticationToken` 스타일의 인증을 지원한다고 명시합니다.


## `Authentication` 구현체
- Spring Security에서 **사용자 인증 정보를 표현하는 클래스**입니다. 이 인터페이스는 사용자의 자격 증명(예: 사용자 이름, 비밀번호) 및 인증 상태(예: 인증 여부, 권한 등)를 포함합니다.
- `Authentication` 인터페이스는 **Spring Security 내 모든 토큰 유형의 부모**입니다. 여러 구현 클래스가 존재합니다.

- `UsernamePasswordAuthenticationToken` : 사용자가 로그인 페이지를 통해 자격 증명을 전송할 때 사용됩니다.
- `TestingAuthenticationToken` : 단위 테스트 시나리오에서 사용됩니다. 실제 인증 없이 동일한 객체를 반환합니다.
- `RememberMeAuthenticationToken`, `OidcLogoutAuthenticationToken` 등 다양한 인증 스타일이 존재합니다.


## ProviderManager의 역할 ✔
`ProviderManager`는 **여러 `AuthenticationProvider`를 반복하여 인증을 시도**합니다.(`authenticate` 메소드 참고) 각 `AuthenticationProvider`의 `supports()` 메소드를 호출하여 지원 여부를 확인합니다. 지원하는 경우 `authenticate()` 메소드를 호출하여 인증을 진행합니다.


# 47. 우리 애플리케이션 내에서 AuthenticationProvider 구현 및 사용자 정의하기
자체 **사용자 지정 `AuthenticationProvider`를 구현**해 보겠습니다.

## 커스텀 AuthenticationProvider 생성
- `AuthenticationProvider` 인터페이스를 구현하고, IntelliJ에서 메소드 재정의 요청을 수락하여 두 메소드를 구현합니다.
- 클래스를 Spring Bean으로 만들기 위해 `@Component` 어노테이션 추가.
```java
@Component  
@RequiredArgsConstructor  
public class EazyBankProdUsernamePwdAuthenticationProvider implements AuthenticationProvider {  
  
    private final UserDetailsService userDetailsService;  
    private final PasswordEncoder passwordEncoder;  
  
    @Override  
    public Authentication authenticate(Authentication authentication) throws 
    }  
  
    @Override  
    public boolean supports(Class<?> authentication) {  
        // 
    }  
}
```


## supports() 메소드 구현✔
- 기본적으로 `DaoAuthenticationProvider`의 `supports()` 메소드 로직을 참고합니다.
- **인증 객체의 타입을 지원하는지 여부를 확인**합니다.
```java
@Override  
public boolean supports(Class<?> authentication) {  
    return (UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication));  
}
```


## authenticate() 메소드 구현 ✔
- **입력 정보 가져오기** : `authentication`로 입력 정보 가져오기
- **사용자 세부 정보 로드**: `EazyBankUserDetailsService`를 사용하여 사용자 세부 정보를 로드합니다.
- **비밀번호 비교**: `PasswordEncoder` Bean을 사용하여 입력된 비밀번호와 저장된 비밀번호를 비교합니다.
- **예외 처리**: 비밀번호가 일치하지 않으면 `BadCredentialsException`을 던지고, 사용자가 존재하지 않으면 `UsernameNotFoundException`을 던집니다.

```java
@Override  
public Authentication authenticate(Authentication authentication) throws AuthenticationException {  
    String username = authentication.getName();  
    String pwd = authentication.getCredentials().toString();  
    UserDetails userDetails = userDetailsService.loadUserByUsername(username);  
    if (passwordEncoder.matches(pwd, userDetails.getPassword())) {  
        // 추가 검증 로직 작성 가능
        return new UsernamePasswordAuthenticationToken(username,pwd,userDetails.getAuthorities());  
    }else {  
        throw new BadCredentialsException("Invalid password!");  
    }  
}
```
- 필요에 따라 추가 검증 로직을 `authenticate()` 메소드 내에 작성할 수 있습니다. (예: 나이 확인)


## 디버깅
- 브라우저에서 API에 **로그인 시도 시 `supports()` 메소드와 `authenticate()` 메소드에서 중단점을 설정하여 디버깅**합니다.
- 사용자 이름과 비밀번호가 올바르게 로드되고 비밀번호 비교가 성공하면 성공적인 응답을 반환합니다.

## Spring Security 시퀀스 흐름 ✔
- 사용자 정의 `AuthenticationProvider`가 `UserDetailsService` 구현과 `PasswordEncoder`의 도움을 받아 **사용자 세부 정보를 로드하고 비밀번호를 검증하는 흐름을 확인**합니다.

▶ 시퀀시 흐름
![[Spring_Security_48.png]]

▶ 시퀀시 흐름
![[Spring_Security_49.png]]


# 48. 프로파일을 사용한 환경별 보안 구성 - 1부
## 다양한 환경
실시간 프로젝트에서는 코드가 여러 환경에 배포됩니다.

▶ **프로덕션 환경**
- 프로덕션 환경: 실제 운영 환경

▶ **비프로덕션 환경**(=하위환경)
애플리케이션 테스트나 시나리오 테스트가 이루어지는 환경을 의미합니다. 
- UAT (사용자 수용 테스트): 최종 사용자가 테스트하는 환경
- SIT (시스템 통합 테스트): 시스템 통합 테스트가 이루어지는 환경
- 개발 환경: 개발자가 변경 사항을 테스트하는 환경


## 환경설정을 하는 이유?
모든 사용자는 시스템에 들어가기 위해 자격 증명을 입력해야 하며, 로그인 페이지에서 사용자 이름과 비밀번호를 입력하게 됩니다. Spring Security가 이 인증 과정을 처리합니다.

많은 조직에서 **하위 환경에서는 비밀번호를 기억할 필요 없이 테스트를 진행할 수 있도록 유연성**을 제공합니다. 이 경우,테스트를 용이하게 하기 위해 하위 환경에서는 사용자 이름만 요구하고 비밀번호는 아무 값이나 입력할 수 있도록 설정하는 경우가 많습니다. 


# 49. 프로파일을 사용한 환경별 보안 구성 - 2부
애플리케이션 내에서 프로파일 개념을 구현하고 기본 사항을 설명하겠습니다.

## 프로파일이란?
Spring Boot에서 **프로파일을 사용하면 환경에 따라 특정 속성 집합이나 Bean 집합을 활성화**할 수 있습니다. 개발자들은 각 환경에 대한 프로파일을 생성합니다:


## 기본 프로파일
현재 프로젝트에는 **`application.properties`라는 단일 속성 파일**이 있습니다. Spring Boot는 기본적으로 **`default` 프로파일을 로드**하며, 활성 프로파일이 설정되지 않으면 기본 프로파일로 돌아갑니다.


## 프로파일 설정하기
1) 공통 프로파일
모든 환경에서 공통으로 사용하는 설정을 포함합니다.
```properties
# application-common.properties
spring.application.name=${SPRING_APP_NAME:eazybankbackend}

spring.datasource.url=jdbc:mysql://${DATABASE_HOST:localhost}:${DATABASE_PORT:3306}/${DATABASE_NAME:eazybank}
spring.datasource.username=${DATABASE_USERNAME:root}
spring.datasource.password=${DATABASE_PASSWORD:root}

spring.jpa.show-sql=${JPA_SHOW_SQL:false}
spring.jpa.properties.hibernate.format_sql=${HIBERNATE_FORMAT_SQL:false}

logging.pattern.console=${LOGPATTERN_CONSOLE:%green(%d{HH:mm:ss.SSS}) %blue(%-5level) %red([%thread]) %yellow(%logger{15}) - %msg%n}
```

2) dev 프로파일
개발 환경에 대한 설정을 포함합니다.
```properties
# application-dev.properties
spring.profiles.active=default
logging.level.org.springframework.security=${SPRING_SECURITY_LOG_LEVEL:TRACE}

# JPA 및 보안 로깅 수준 설정
spring.jpa.show-sql=${JPA_SHOW_SQL:true}
spring.jpa.properties.hibernate.format_sql=${HIBERNATE_FORMAT_SQL:true}

```

3) prod 프로파일
프로덕션 환경에 특화된 설정을 포함합니다.
```properties
# application-prod.properties
spring.config.activate.on-profile=prod
logging.level.org.springframework.security=${SPRING_SECURITY_LOG_LEVEL:ERROR}

# JPA 및 보안 로깅 수준 설정
spring.jpa.show-sql=${JPA_SHOW_SQL:false}
spring.jpa.properties.hibernate.format_sql=${HIBERNATE_FORMAT_SQL:false}

```

4) 메인 프로파일
공통 설정 파일을 가져오고, 환경별 프로파일 활성화합니다.
```properties
# application.properties
spring.config.import=classpath:application-common.properties

# 환경별 프로파일 활성화
spring.profiles.active=default
```


## 환경 변수 사용
- Spring Boot는 환경 변수를 우선시하며, 환경 변수가 정의되지 않은 경우 `application.properties`의 값을 사용합니다.
- 실행 구성 수정(Modify Run Configuration)을 선택합니다.

![[Pasted image 20240929230215.png]]
- `prod`, `default` 입력해서 운영환경 바꿔보기


# 50. 프로파일을 사용한 환경별 보안 구성 - 3부
 - Spring Boot 프로파일을 사용하여 특정 Bean을 활성화하는 방법에 대해 설명했습니다.


## 특정환경에서 클래스 활성화하기
- `@Profile` 어노테이션을 사용하여 `prod` 프로파일에서만 활성화되도록 설정합니다. `!prod`를 사용하면 프로파일 환경이 아닌 상황에서만 활성화 됩니다.

![[Spring_Security_50.png]]


## prod 환경 비즈니스 로직 변경
- `EazyBankProdUsernamePwdAuthenticationProvider` 내에서 비밀번호 검증을 위한 `if` 조건문을 제거합니다. 이로 인해 **비밀번호 검증이 이루어지지 않게 됩니다.**
- 이로 인해, 브라우저에서 사용자 이름 (`smith@example.com`)과 임의의 비밀번호 (`12345`)를 입력하여 로그인 시도. **비밀번호가 유효하지 않더라도 성공적인 응답**을 받습니다.

```java
@Override  
public Authentication authenticate(Authentication authentication) throws AuthenticationException {  
	String username = authentication.getName();  
	String pwd = authentication.getCredentials().toString();  
	UserDetails userDetails = userDetailsService.loadUserByUsername(username);  
	return new UsernamePasswordAuthenticationToken(username,pwd,userDetails.getAuthorities());  
}
```


## prod 프로파일 활성화한 경우
- 애플리케이션을 중지하고 `prod` 프로파일을 활성화합니다.
- 브라우저에서 동일한 사용자 이름을 입력하고 유효한 비밀번호 (`easybytes@12345`)로 로그인 시도.
- 올바른 비밀번호를 입력해야만 성공적인 응답을 받을 수 있습니다.


## 조건부 보안 구성
- QA 담당자와 비즈니스 분석가에게 **여러 사용자의 비밀번호를 기억하게 할 필요가 없습니다**. 프로파일에 따라 조건부 보안 구성을 할 수 있는 솔루션을 제공함으로써 편리함을 극대화합니다.

