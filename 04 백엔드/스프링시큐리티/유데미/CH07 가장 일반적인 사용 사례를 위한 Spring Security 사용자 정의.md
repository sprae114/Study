# 요약
- HTTP/HTTPS 트래픽를 구별해서 허용할 수 있음(p.51)
- 시큐리티에서 처리하는 2가지 예외에 대해서 알 수 있음(p.52)
- 시큐리티에서 처리하는 2가지 예외를 처리 할 수 있음(p.53~55)
- 세션 세부 설정을 할 수 있음(p.56~58)
- 인증 성공/실패 이벤트가 발생했을때, 커스텀할 수 있음(p.59~60)
- 커스텀 로그인페이지를 만들어 로그인을 수행할 수 있음(p.61~64)
- 로그아웃을 커스텀 구현할 수 있음(p.65)
- Thymleaf에 인증기능을 추가 할 수 있음(p.66)
- SecurityContext에 대해서 이해할 수 있음(p.67)
- 인증된 사용자 정보를 가져오는 2가지 방법을 구현 할 수 있음(p.68)

# 51. 스프링 시큐리티를 사용하여 HTTPS 트래픽만 허용하기

## HTTPS가 필요한 이유 ✔
- 웹사이트나 모바일 애플리케이션을 안전하게 사용하기 위해서는 HTTPS 프로토콜을 통해 요청을 수락하고 처리해야 합니다. 
- 대부분의 조직은 백엔드 애플리케이션이 HTTP가 아닌 HTTPS만 수락하도록 설정합니다. 
- HTTPS는 **데이터의 보안을 보장**하며, 사용자가 신용카드 정보를 안심하고 제출할 수 있도록 합니다.

## HTTP와 HTTPS의 차이 ✔
- **HTTP**: **평문 요청**, 보안이 필요 없는 데이터 전송.
- **HTTPS**: **암호화된 요청**, 보안이 필요한 데이터 전송.

## 코드 적용하기 ✔
1) HTTP + HTTPS 
기본값이라 설정 안해도 됨.

2) HTTP만
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    http.requiresChannel(rcc -> rcc.anyRequest().requiresInsecure()) // Only HTTP
    ...
}
```

3) HTTPS만
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    http.requiresChannel(rcc -> rcc.anyRequest().requiresSecure()) // Only HTTPS
    ...
}
```
![[Spring_Security_51.png]]


# 52. 스프링 시큐리티 프레임워크에서 예외 처리하기
Spring Security에서 발생하는 **예외를 사용자 정의하는 방법**에 대해 설명하겠습니다.


## Security 예외 처리 이해 ✔
![[Spring_Security_52.png]]
Spring Security는 **`ExceptionTranslationFilter`에 의해서 예외를 처리**합니다. 
Spring Security는 보안 관련 예외에 중점을 두고 있으며, 다음과 같은 두 가지 주요 예외 유형이 있습니다.

▶ `AuthenticationException`
- **상태 코드 401**: 사용자나 클라이언트 애플리케이션이 **인증되지 않았음**을 나타냅니다.
- **`AuthenticationEntryPoint` 인터페이스**에 의해서 구현됩니다.
- 예: `BadCredentialsException`, `UsernameNotFoundException`

▶ `AccessDeniedException`
- **상태 코드 403**: 사용자가 인증되었지만, 보안된 **API에 접근할 권한이 없음**을 의미합니다.
- **`AccessDeniedHandler` 인터페이스**에 의해 구현됩니다.


## 디버그
`4:56`초 강의로 확인하기
- `AuthenticationException`은 기본값으로 `BasicAuthenticationEntryPoint` 사용합니다.
- `AccessDeniedException`은 기본값으로 `AccessDeniedHandlerImpl` 사용합니다.


# 53. 사용자 정의 AuthenticationEntryPoint 정의 - 1부
`AuthenticationEntryPoint`를 정의하는 방법을 설명하겠습니다.

## AuthenticationEntryPoint 인터페이스 이해 ✔
- `AuthenticationEntryPoint`는 Spring 애플리케이션에서 **인증되지 않은 접근 시도를 처리하는 핸들러 역할**을 합니다. 사용자가 인증 없이 사이트나 API의 일부에 접근하려고 할 때, **어떻게 응답할지를 결정**합니다.
- 이 진입점은 **HTTP 기본 인증 시나리오에서만 호출**됩니다.
![[Spring_Security_53.png]]


## 기본 로직 설명
- `BasicAuthenticationEntryPoint`
Spring Security에서 **인증 실패 시 HTTP 응답을 설정**하는 메서드입니다.
```java
@Override  
public void commence(HttpServletRequest request, HttpServletResponse response,  
       AuthenticationException authException) throws IOException {  
       //헤더 설정
    response.setHeader("WWW-Authenticate", "Basic realm=\"" + this.realmName + "\"");  
    //응답 오류 전송
    response.sendError(HttpStatus.UNAUTHORIZED.value(), HttpStatus.UNAUTHORIZED.getReasonPhrase());  
}
```


## 사용자 정의 AuthenticationEntryPoint 구현✔
1) **예외의 응답 처리**
사용자 정의 JSON응답을 생성하기 위해 비즈니스 로직을 구현합니다.
```java
@Override  
public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException)  
        throws IOException, ServletException {  
    // 동적 변수 할당
    LocalDateTime currentTimeStamp = LocalDateTime.now();  
    String message = (authException != null && authException.getMessage() != null) ? authException.getMessage()  
            : "Unauthorized";  
    String path = request.getRequestURI();  

	// 응답 구성 만들기
    response.setHeader("eazybank-error-reason", "Authentication failed");  
    response.setStatus(HttpStatus.UNAUTHORIZED.value());  
    response.setContentType("application/json;charset=UTF-8");  
    
    // JSON 응답 만들기
    String jsonResponse =  
            String.format("{\"timestamp\": \"%s\", \"status\": %d, \"error\": \"%s\", \"message\": \"%s\", \"path\": \"%s\"}",currentTimeStamp, HttpStatus.UNAUTHORIZED.value(), HttpStatus.UNAUTHORIZED.getReasonPhrase(),  message, path);  
    response.getWriter().write(jsonResponse);  
}
```

2) **Spring Security에 진입점 전달**
- `ProjectSecurityConfig`로 이동합니다. `httpBasic()` 메소드에서 **기본값을 제거하고 사용자 정의를 추가**합니다
```java
httpBasic(hbc -> hbc.authenticationEntryPoint(new CustomBasicAuthenticationEntryPoint()));
```


## 빌드 및 테스트
- 변경 사항을 저장하고 빌드를 수행합니다. `CustomBasicAuthenticationEntryPoint`와 기본 인증 진입점에 중단점을 설정합니다.
- 유효하지 않은 사용자 이름으로 API 요청을 보내고 401 오류가 발생하는지 확인합니다. 응답에서 사용자 정의 JSON 메시지를 확인합니다.

▶ 기존 JSON
```json
{
	"timestamp": "2024-09-29T17:47:07.497+00:00",
	"status": 401,
	"error": "Unauthorized",
	"message": "Unauthorized",
	"path": "/myAccount"
}
```


▶ 바뀐 JSON
```json
{
	"timestamp": "2024-09-30T02:52:44.716822200",
	"status": 401,
	"error": "Unauthorized",
	"message": "User details not found for the user: smith@example.com123",
	"path": "/myAccount"
}
```


# 54. 사용자 정의 AuthenticationEntryPoint 정의 - 2부
`httpBasic()` 설정을 사용하여 **전역적으로 설정하는 방법**에 대해 설명하겠습니다.

## 전역으로 설정하는 이유는?
- Spring Security는 **여러 다른 시나리오에서 `AuthenticationException`을 발생시킬 수 있기 때문에 전역 설정이 유용**할 수 있습니다. ( `formLogin`을 설정할 수는 없습니다.)
- **일관된 사용자 경험**: 모든 예외를 중앙에서 처리함으로써, 사용자에게 일관된 오류 메시지나 페이지를 제공할 수 있습니다.


## 폼 로그인 설정은?
- `authenticationEntryPoint()` 메소드를 사용할 수 있지만, `formLogin`에서는 사용할 수 없습니다.이는 UI 애플리케이션을 사용하는 **최종 사용자가 JSON 응답이나 응답 헤더를 필요로 하지 않기 때문**입니다.
- `forlogin`에서 사용할 수 있는 `defaultSuccessUrl()`, `failureHandler()`, `loginProcessingUrl()`, `failureUrl()`과 같은 많은 다른 메소드가 있습니다


## 전역 설정 사용 ✔
- `http` 변수를 사용하여 `exceptionHandling()` 메소드를 호출합니다. 이 메소드에 람다 표현식을 사용하여 사용자 정의 설정을 전달할 수 있습니다.
```java
http.exceptionHandling(ehc -> 
    ehc.authenticationEntryPoint(new CustomBasicAuthenticationEntryPoint())
);
````


# 55. 사용자 정의 AccessDeniedHandler 정의하기
403 관련 예외를 처리하기 위한 **사용자 정의 `AccessDeniedHandler`를 구현하는 방법**을 설명하겠습니다.

## 403 예외 설명
- **액세스 거부 예외**에 해당하며, Spring Security 프레임워크 내에서 여러 하위 클래스가 존재합니다.
- 예외 예시: `AuthorizationDeniedException`, `CsrfException`, `InvalidCsrfTokenException`

## AccessDeniedHandler 인테페이스 설명 ✔
- 이 인터페이스는 **접근이 거부될 때 호출되는 핸들러를 정의**합니다.
![[Spring_Security_56.png]]


## 사용자 정의 AccessDeniedHandler 구현✔
1) 응답 처리
이전의 **`사용자 정의 AuthenticationEntryPoint`에서 처럼 응답을 만들기 원합**니다. 그래서 비즈니스 내용을 복사하고 재정의하겠습니다.
```java
@Override  
public void handle(HttpServletRequest request, HttpServletResponse response,  
        AccessDeniedException accessDeniedException) throws IOException, ServletException {  
    // 동적 값 할당
    LocalDateTime currentTimeStamp = LocalDateTime.now();  
    String message = (accessDeniedException != null && accessDeniedException.getMessage() != null) ?  
            accessDeniedException.getMessage() : "Authorization failed";  
    String path = request.getRequestURI();  

	// 응답 구성 만들기
    response.setHeader("eazybank-denied-reason", "Authorization failed");  
    response.setStatus(HttpStatus.FORBIDDEN.value());  
    response.setContentType("application/json;charset=UTF-8");  
    
    // JSON 응답 만들기  
    String jsonResponse =  
            String.format("{\"timestamp\": \"%s\", \"status\": %d, \"error\": \"%s\", \"message\": \"%s\", \"path\": \"%s\"}",  
                    currentTimeStamp, HttpStatus.FORBIDDEN.value(), HttpStatus.FORBIDDEN.getReasonPhrase(),  
                    message, path);  
    response.getWriter().write(jsonResponse);  
}
```


2) Spring Security 설정에 추가
- `accessDeniedHandler()` 메소드를 사용하여 `CustomAccessDeniedHandler` 객체를 전달합니다.
```java
http.exceptionHandling(ehc -> 
    ehc.accessDeniedHandler(new CustomAccessDeniedHandler())
);
```


## AccessDeniedPage 설정
- `exceptionHandling` 구성에서 `accessDeniedPage()` 메소드를 호출하여 **사용자 정의 페이지를 설정할 수 있습니다.**
- 예를 들어, 페이지 이름을 `"/denied"`로 설정하면, 애플리케이션 내에서 403 오류가 발생할 때마다 최종 사용자는 `/denied` 경로로 리다이렉션됩니다. (cotroller 구성되어 있어야함.)
- 두 가지 구성을 모두 정의하는 경우는 REST API만 지원하는 애플리케이션에만 적합합니다. (**UI와 백엔드 로직안에 Spring인 경우**)
```java
http.exceptionHandling(ehc -> ehc.accessDeniedHandler(new CustomAccessDeniedHandler())
					   .accessDeniedPage("/denied"));
```


# 56. 세션 타임아웃 및 유효하지 않은 세션 구성
**세션 타임아웃을 설정하는 방법에** 대해 설명하겠습니다.

## 세션 타임아웃 기본 설정
- 기본적으로 로그인 후 생성되는 모든 세션의 타임아웃은 **30분**으로 설정되어 있습니다.사용자가 30분 동안 활동하지 않으면 세션이 무효화됩니다.
- 무효화시, **로그인 페이지로 리다이렉션**됩니다.


## 사용자 정의 세션 타임아웃 설정 ✔
- 타임아웃은 **2분 이상**이어야 하며, 10초, 30초 또는 1분은 설정할 수 없습니다.
- 타임아웃 시간을 사용자 정의하려면 `application.properties` 파일에 다음 속성을 추가합니다
```properties
server.servlet.session.timeout=20m
```
- 이 설정은 기본 프로파일에서 20분으로 설정하였으며, 사용자가 20분 동안 활동하지 않으면 세션이 만료됩니다.


## 사용자 세션 리다이렉션 처리 ✔
- 세션 타임아웃으로 인해 리다이렉션되는 경우 사용자에게 적절한 메시지를 전달하기 위해 `sessionManagement()` 메소드를 사용합니다.
- `invalidSessionUrl()` 메소드를 호출하여 **사용자가 세션이 유효하지 않을 때 리다이렉션될 경로를 설정**합니다. (**JESSION값이 유효하지 않거나 타임아웃이 됐을때**)
```java
http.sessionManagement().invalidSessionUrl("/invalidSession");
```
- **`/invalidSession` 경로에 대한 MVC 경로가 구성**되어 있어야 합니다. 그렇지 않으면 리다이렉션이 의미가 없게 됩니다.


# 57. 동시 세션 제어 구성
최종 사용자가 가질 수 있는 **동시 세션을 제어하는 방법**에 대해 설명하겠습니다.

## 기본 동시 세션 설정
- 기본적으로 Spring Security는 최종 사용자가 **여러 개의 동시 세션을 가질 수 있도록 제한**하지 않습니다.
- 그러나 실제 애플리케이션에서는 동시 세션을 하나만 허용하거나, 특정 수(2개, 3개 등)로 제한하고 싶을 수 있습니다.


##  동시 세션 제어 구현 ✔
- `sessionManagement()` 메소드 아래에 `maximumSessions(1)`을 설정하여 **동시 세션 수를 1로 제한**합니다.
```java
http.sessionManagement(smc -> smc
					   .invalidSessionUrl("/invalidSession")
					   .maximumSessions(1));
```


## 동시 세션 테스트
- Postman에서 사용자의 세션을 생성한 후, **동일한 사용자로 브라우저에서 로그인 시도**합니다. 두 번째 세션 생성 시도 시 **첫 번째 세션이 무효화되고, 로그인 페이지로 리다이렉션**됩니다.


## 세션 최대 갯수 제한 구현 ✔
- `maxSessionsPreventsLogin(true)`를 설정하면, **최대 세션 수에 도달했을 때 새로운 로그인 시도가 실패**합니다.
- 이 메소드는 기본값이 false이므로, true로 변경하여 동작을 수정할 수 있습니다.
```java
http.sessionManagement(smc -> smc
					   .invalidSessionUrl("/invalidSession")
					   .maximumSessions(1)
					   .maxSessionsPreventsLogin(true));
```


## 세션 최대 갯수 제한 테스트
- Postman에서 사용자의 세션을 생성한 후, **동일한 사용자로 브라우저에서 로그인 시도**합니다. 두 번째 세션 생성 시도 시 `Maximum sessions of 1 for this principal exceeded` 유효성 검사 오류가 나타납니다.


## 세션 만료 시 리다이렉션 구현 ✔
- `expiredUrl("/invalidSession")` 메소드를 사용하여 세**션이 만료될 때 최종 사용자를 리다이렉션할 URL을 설정**할 수 있습니다.
```java
http.sessionManagement(smc -> smc
					   .invalidSessionUrl("/invalidSession")
					   .maximumSessions(1)
					   .maxSessionsPreventsLogin(true)
					   .expiredUrl("/invalidSession"));
```


## 세션 만료 시 리다이렉션 테스트
- 로그인 만료 시간후, **`/login`으로 리다이렉션에서 `/invalidSession`로 바꼇습니다.**


# 58. 스프링 시큐리티를 사용한 세션 고정 공격 방어
**세션 고정과 세션 하이재킹**이라는 두 가지 중요한 보안 주제에 대해 설명하겠습니다.

## 세션 하이재킹 ✔
- 세션 하이재킹은 공격자가 웹 애플리케이션의 **세션 ID를 탈취하여 사용자의 신분으로 행동하는 공격**입니다.

▶ 위험 요소
- 세션 ID가 URL에 포함되거나 쿠키로 저장될 수 있습니다.
- 공공장소의 컴퓨터 사용 시 세션 ID가 탈취될 위험이 있습니다.

▶ 예방 조치
- **HTTPS 사용**: 네트워크 트래픽을 암호화하여 세션 ID를 보호합니다.
- **짧은 세션 타임아웃**: 세션 ID의 유효 기간을 짧게 설정하여 공격 가능성을 줄입니다.
- 공용 컴퓨터 사용 체크박스: 사용자가 공용 컴퓨터를 사용하는지 확인합니다.


## 세션 고정 공격 ✔
- 세션 고정 공격은 **해커가 세션 ID를 조작하여 사용자가 해당 세션 ID를 사용하도록 유도하는 공격**입니다.

▶ 시나리오
- 해커가 자신의 세션 ID를 포함한 링크를 정직한 사용자에게 보냅니다.
- 사용자가 **해당 링크를 클릭하면 해커의 세션 ID로 로그인하게 되어 해커가 사용자의 세션에 접근**할 수 있습니다.


## 세션 고정 공격 처리 대처 전략 ✔
-  Spring Security의 기본 동작은 `changeSessionId` 입니다.
- `SessionManagementConfigurer` 클래스 안에 설명되어 있습니다.
![[Spring_Security_61.png]]

▶ 세 가지 전략
1. `changeSessionId`
- 설명: 사용자가 로그인할 때 **현재 세션의 ID를 새로운 ID로 변경**합니다. 이 방법은 기존 세션에 대한 공격자가 이미 알고 있을 수 있는 세션 ID를 무효화하고, 새로운 세션 ID를 생성하여 보안을 강화합니다.
- 장점: 간단하고 효과적으로 세션 고정 공격을 방지할 수 있습니다. 사용자는 **이전 세션에서의 모든 정보를 잃게** 되지만, 새로운 세션으로 안전하게 이동합니다.

2. `newSession`
- 설명: 사용자가 로그인할 때 **완전히 새로운 세션을 생성**합니다. 이 방법은 기존 세션을 무효화하고, 모든 세션 속성을 새로운 세션으로 이전하지 않습니다.
- 장점: 기존 세션의 모든 정보를 잃더라도, 새로운 세션이 생성되므로 **세션 고정 공격에 대한 저항력**이 높습니다. 이는 특히 세션 속성에 민감한 경우 유용합니다.

3. `migrateSession`
- 설명: 기존 세션의 속성을 **새로운 세션으로 이전하면서 세션 ID를 변경**합니다. 이 방법은 기존 세션의 **데이터를 유지**하되, 새로운 ID로 변경하여 세션 고정 공격을 방지합니다.
- 장점: **사용자 경험을 유지하면서 세션 고정 공격을 방지**할 수 있습니다. 기존 세션의 속성을 보존하므로, 사용자는 로그인 후에도 이전 상태를 유지할 수 있습니다.


## 세션 고정 공격 처리 대처 구현 ✔
`sessionFixation()`를 이용해서 고정 공격 처리 대처 구현했습니다. 기본값은 `changeSessionId`입니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    http.sessionManagement(smc -> smc.sessionFixation(sfc -> sfc.migrateSession())  
                    .invalidSessionUrl("/invalidSession")  
                    .maximumSessions(3)  
                    .maxSessionsPreventsLogin(true)
                    )
        ...
```


# 59. 인증 이벤트 수신 - 이론
Spring Security에서 **인증 이벤트를 처리하는 방법**에 대해 설명하겠습니다. 인증 성공 및 실패 시 발생하는 이벤트를 활용하여 비즈니스 로직을 실행할 수 있습니다.


## 인증 이벤트의 중요성 ✔
- **인증 성공 시 최종 사용자에게 알림 이메일을 보내거나, 인증 실패 시도에 대한 로그를 기록하는 등의 요구 사항이 자주 발생**합니다.
- `AuthenticationSuccessEvent`: 로그인 작업이 성공할 때 게시됩니다.
- `AuthenticationFailureEvent`: 로그인 작업이 실패할 때 게시됩니다.


## 이벤트는 어떻게 게시될까?
- 이벤트는 Spring Security의 **`DefaultAuthenticationEventPublisher` 클래스에 의해 게시**됩니다. 이러한 이벤트에 기반한 비즈니스 로직을 실행하기 위해 이벤트 리스너를 작성할 수 있습니다.


# 60. 인증 이벤트 수신 - 데모
Spring Security에서 **인증 이벤트를 처리하는 방법**에 대해 설명하겠습니다. 인증 성공 및 실패 이벤트를 수신하여 비즈니스 로직을 실행할 수 있습니다.

## Security 이벤트가 어떻게 게시되는 코드 디버깅
`0:12`부터 보기


## 이벤트 리스너 구현 ✔
- `@Component`로 `AuthenticationEvents`를 **등록**해야합니다.
- `@EventListener` 어노테이션을 사용하여 **Spring Security의 이벤트를 수신**합니다. 이를 통해 인증 성공 및 실패 이벤트를 지속적으로 수신할 수 있습니다.
- 메소드 이름은 자유롭게 지정할 수 있지만, **입력 매개변수**는 각각 `AuthenticationSuccessEvent`와 `AuthenticationFailureEvent`를 받아야 합니다.
```java
@Component  
@Slf4j  
public class AuthenticationEvents {  
  
    @EventListener
	public void onSuccess(AuthenticationSuccessEvent event) {
	    String username = event.getAuthentication().getName();
	    // 로그 기록
	    logger.info("사용자 로그인 성공: " + username);
	}
	
	@EventListener
	public void onFailure(AuthenticationFailureEvent event) {
	    String username = event.getAuthentication().getName();
	    String errorMessage = event.getException().getMessage();
	    // 로그 기록
	    logger.error("로그인 실패: " + username + ", 이유: " + errorMessage);
	}
}
```


## 디버깅 및 로그 확인
1. 인증 실패 시나리오에서 잘못된 자격 증명을 사용하여 API를 호출합니다. `onFailure()` 메소드에서 중단점이 멈추며, 실패 이벤트의 세부 사항을 확인합니다.

2. 인증 성공 시나리오에서 유효한 자격 증명을 사용하여 API를 호출합니다. `onSuccess()` 메소드에서 중단점이 멈추고, 성공 메시지를 로그로 확인합니다.


# 61. 사용자 정의 폼 로그인 구성 - 1부
 Spring Security의 `formLogin()` 사용자 정의 설정에 대해 설명하겠습니다. 이 설정을 통해 사용자 **인증 시 커스터마이즈된 로그인 페이지와 성공적인 인증 후의 리다이렉션을 처리**할 수 있습니다.

## formLogin() 설정의 필요성 
- 현재는 Security에서 제공하는 기본 로그인 페이지를 이용해서 로그인을 하고 있습니다.
- 커스텀을 통해서 서비스에서 자체 **로그인 페이지를 표시**하고, **인증 성공 시 다른 페이지로 리다이렉션**할 수 있습니다.


## EazySchool 애플리케이션 소개
- EazySchool 애플리케이션은 Spring MVC와 HTML 페이지를 사용하여 구축된 학교 웹 애플리케이션입니다. 이 애플리케이션을 통해 `formLogin()` 관련 설정을 쉽게 이해할 수 있습니다.

▶ UI 페이지 구성
- 홈페이지: 학교의 기본 정보와 과정을 보여줍니다.
- 로그인 페이지: Spring Security에서 제공하는 기본 로그인 페이지로 리다이렉션됩니다.
- 대시보드: 인증된 사용자에게만 제공되며, 환영 메시지와 함께 사용자 권한을 표시합니다.
- 로그아웃 페이지: 로그아웃 후 '로그아웃되었습니다'라는 메시지를 보여줍니다.
- 야간/주간 모드: 사용자 설정에 따라 테마를 변경할 수 있습니다.



# 62. 사용자 정의 폼 로그인 구성 - 2부
`formLogin()` 설정을 통해 **사용자 정의 로그인 페이지를 구현하는 방법**에 대해 설명하겠습니다.

## 애플리케이션 설정
▶ `application.properties`
Spring Security의 로깅 패턴과 로깅 레벨 외에는 중요한 속성이 없습니다. 데이터베이스 연결이 필요 없는 간단한 애플리케이션입니다.

▶ `ProjectSecurityConfig`
- 단일 `ProjectSecurityConfig` 클래스를 사용하며, CSRF 공격을 비활성화했습니다.
- 다양한 경로에 대해 `permitAll()`을 설정하여 모든 사용자가 접근할 수 있도록 구성했습니다.
- 대시보드와 같은 특정 경로는 나중에 `authenticated()`로 변경하여 보안을 적용할 예정입니다.

▶Static Resources 및 Controller 설정
- **정적 자산**: `assets/` 경로에 대한 접근을 허용하여 CSS, 이미지, JavaScript 파일을 사용할 수 있도록 설정했습니다.
- **HTML 페이지**: 모든 HTML 파일은 `templates` 폴더에 존재하며, 각 페이지는 공통된 `header.html`과 `footer.html`을 포함합니다.


## 사용자 formLogin() 만들기 ✔
1) **대시보드 보호** : 대시보드 페이지를 보안 페이지로 만들기 위해 `permitAll()`을 `authenticated()`로 변경합니다.

2) **경로 접근 허용** : `/login/**` 경로에 대한 접근할 수 있도록 설정했습니다.

3) **사용자 정의 로그인 페이지** : 기본 로그인 페이지 대신 커스터마이즈된 로그인 페이지를 사용하고자 합니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	...
    http.formLogin(flc -> flc.loginPage("/login"))  //사용자 정의 로그인 페이지
    ...
    return http.build();  
}
```

4) **컨트롤러 작성** : 컨트롤러 작성해서 커스터마이즈된 로그인 페이지 매핑하도록 합니다.
```java
@RequestMapping(value = "/login", method = {RequestMethod.GET})  
public String displayLoginPage() {  
    return "login.html";  
}
```

5) **커스터마이즈된 로그인 페이지** : 커스터마이즈된 로그인 페이지만듭니다.


## 사용자 formLogin() 만들면 생기는일
`formLogin()`만 지정된 경우의 값을 **업데이트하면 다른 여러 기본값에도 영향**을 미칩니다.
- `/login GET` : 기본 로그인 양식
- `/login POST` : 자격 증명을 처리하고 유효한 경우 사용자를 인증
- `/login?error GET` : 인증 시도에 실패하면 여기로 리다이렉션
- `/login?logout GET` : 성공적으로 로그아웃한 후 여기로 리다이렉션 


## 사용자 formLogin() 테스트
- 대시보드 접근 시 사용자 정의 로그인 페이지로 리다이렉션되는 것을 확인합니다.


# 63. 사용자 정의 폼 로그인 구성 - 3부
인증 후의 리다이렉션을 설정하는 방법과 파라미터 변경하여 로그인 하는 방법을 배웠습니다.

## 인증 성공 후 리다이렉션 설정 ✔
- Spring Security 설정을 통해 인증 성공 시 기본 리다이렉션 URL을 설정할 수 있습니다.
- **기본값은 최종 사용자가 이전에 접근했던 페이지로 이동**한다는 점입니다.
- `defaultSuccessUrl("/dashboard")`를 사용하여 대시보드를 기본 성공 URL로 설정합니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    ...
    
    http.formLogin(flc -> flc.loginPage("/login").defaultSuccessUrl("/dashboard"))  
	...
    return http.build();  
}
````


## 로그인 과정의 내부 동작 ✔
- `UsernamePasswordAuthenticationFilter` 필터가 사용되며, **로그인 요청을 처리**합니다.
- `attemptAuthentication()` 메소드에서 **사용자 이름과 비밀번호를 추출**합니다.
- **요청에서 사용자 이름과 비밀번호의 매개변수를 확인**합니다.
- 비밀번호와 **매칭하여 로그인** 합니다.
![[Spring_Security_18 2.png]]


## 매개변수 정하는 매커니즘 이해하기
- 기본적으로 `username`과 `password`라는 매개변수를 요구합니다. 
▶ `UsernamePasswordAuthenticationFilter`클래스
```java
public static final String SPRING_SECURITY_FORM_USERNAME_KEY = "username";  //기본값
public static final String SPRING_SECURITY_FORM_PASSWORD_KEY = "password";  //기본값
  
  
private String usernameParameter = SPRING_SECURITY_FORM_USERNAME_KEY;  
private String passwordParameter = SPRING_SECURITY_FORM_PASSWORD_KEY;

//Authentication만들기
@Override  
public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)  
       throws AuthenticationException {  

	...
	
    String username = obtainUsername(request);  
    username = (username != null) ? username.trim() : "";  
    String password = obtainPassword(request);  
    password = (password != null) ? password : "";  
    
	...
}

//파라미터 이름 바꾸는 메소드
@Nullable  
protected String obtainUsername(HttpServletRequest request) {  
    return request.getParameter(this.usernameParameter);  
}

```

▶ 로그인 html
```html
<input type="text" name="username" id="username" placeholder="Username" />
<input type="password" name="password" id="password" placeholder="Password" />
```


## 로그인 사용자 매개변수 구현✔
1) **Parameter설정하기**
`usernameParameter("userid")` 및 `passwordParameter("secretPwd")`를 사용하여 **사용자 정의 매개변수 이름을 설정**할 수 있습니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    ...
    
    http.formLogin(flc -> flc.loginPage("/login")
						    .usernameParameter("userid")
						    .passwordParameter("secretPwd")  
		                    .defaultSuccessUrl("/dashboard"))  
	...
	
    return http.build();  
}
````

2)  **HTML 코드 수정**
`login.html`에서 사용자 이름과 비밀번호 필드의 `name` 및 `id` 속성을 변경하여 일관성을 유지합니다.
```html
<input type="text" name="userid" id="userid" placeholder="Username" />
<input type="password" name="secretPwd" id="secretPwd" placeholder="Password" />
```

## 매개변수 테스트
- `attemptAuthentication` 메소드에서 `username`과 `password`가 바뀐 파라미터로 받는지 확인합니다.


# 64. 사용자 정의 폼 로그인 구성 - 4부
**로그인 실패 및 성공 시 동작을 설정하는 방법**에 대해 설명하겠습니다.

## 기본 로그인 실패 시 리다이렉션 ✔
- 현재 잘못된 자격 증명으로 로그인하면 Spring Security는 **기본값으로 `/login?error` 페이지로 리다이렉션**됩니다. 로그인 실패 시 사용자에게 오류 메시지를 표시하도록 설정할 수 있습니다.
- 기본 로그인에서 사용자 로그인으로 커스텀 했기때문에 `/login?error` 페이지도 설정해야합니다.


## 로그인 failureUrl 구현 ✔
1) failureUrl 설정
`defaultSuccessUrl()` 다음에 `failureUrl("/login?error=true")`을 추가합니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    ...
    
    http.formLogin(flc -> flc.loginPage("/login") //로그인 매핑 페이지
							.defaultSuccessUrl("/dashboard")  //성공시 이동할 URL
							.failureUrl("/login?error=true") //실패시 이동할 URL
	...
    return http.build();  
}
````

2) LoginController 수정
메소드에 `@RequestParam("error")`를 추가합니다. `required` 속성을 `false`로 설정하여 일반 로그인 흐름에서는 오류 요청 매개변수를 보내지 않도록 합니다.
```java
@RequestMapping(value = "/login", method = {RequestMethod.GET})  
public String displayLoginPage(@RequestParam(value = "error", required = false) String error, Model model) {  
    String errorMessge = null;  
    if(null != error) {  
        errorMessge = "Username or Password is incorrect !!";  
    }  
    model.addAttribute("errorMessge", errorMessge);  
    return "login.html";  
}
```

3) html 설정
잘못된 자격 증명으로 로그인하면 오류 메시지가 나타나게 추가하기
```html
...
<form action="/login" method="post" class="signin-form">  
    <div class="col-md-8 login-center input-grids">  
        <li class="alert alert-danger" role="alert" th:if="${!#strings.isEmpty(errorMessge)}"  
            th:text="${errorMessge}"/>  
        ..
    </div>
...
```


## 로그인 failureUrl 테스트
- 로그인 실패시 `/login?error=true` 로 리다이렉트 되는지 확인합니다.
- 잘못된 자격 증명으로 로그인하면 오류 메시지가 표시되는지 확인합니다.


## 핸들러 구현의 필요성 ✔
- 기존에는 성공/실패시 URL매핑으로만 했습니다. 하지만 **로그인 성공/실패시 로그를 남겨달라는 요구사항**이 들어온다면?
- `CustomAuthenticationSuccessHandler`와 `CustomAuthenticationFailureHandler` 클래스를 생성하여 각각 로그인 성공 및 실패 시 동작을 구현합니다.


## 커스텀 성공 핸들러 구현 ✔
1) `CustomAuthenticationSuccessHandler` 구현
로그인 성공 시 오류 메시지를 로그에 기록하고 로그인 페이지로 리다이렉션합니다.
```java
@Component
@Slf4j
public class CustomAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
        log.info("사용자 로그인 성공: {}", authentication.getName());
        response.sendRedirect("/dashboard"); // 대시보드로 리다이렉션
    }
}
```

2) config 등록
로그인 성공시 발생할 handler 등록합니다.
```java
@Configuration  
@RequiredArgsConstructor  
public class ProjectSecurityConfig {
	
	private final CustomAuthenticationSuccessHandler authenticationSuccessHandler;  

	@Bean  
	SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	    ...
	    
	    http.formLogin(flc -> flc.loginPage("/login") //로그인 매핑 페이지
							    //성공시 발생할 Handler
								.successHandler(authenticationSuccessHandler))		
		...
	    return http.build();  
	}
}
````


## 커스텀 실패 핸들러 구현 ✔
1) `CustomAuthenticationFailureHandler` 구현
로그인 실패시 오류 메시지를 로그에 기록하고 로그인 페이지로 리다이렉션합니다.
```java
@Component
@Slf4j
public class CustomAuthenticationFailureHandler implements AuthenticationFailureHandler {
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException {
        log.error("로그인 실패 원인: {}", exception.getMessage());
        response.sendRedirect("/login?error=true"); // 로그인 실패 시 리다이렉션
    }
}
```

2) config 등록
로그인 성공시 발생할 handler 등록합니다.
```java
@Configuration  
@RequiredArgsConstructor  
public class ProjectSecurityConfig {
	
	private final CustomAuthenticationSuccessHandler authenticationSuccessHandler;  

	@Bean  
	SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	    ...
	    
	    http.formLogin(flc -> flc.loginPage("/login") //로그인 매핑 페이지
							    //성공시 발생할 Handler
								.successHandler(authenticationSuccessHandler))		
								//실패시 발생할 Handler
								.failureHandler(authenticationFailureHandler))
		...
	    return http.build();  
	}
}
````


## 핸들러 테스트
- 성공시 `/dashboard`로 리다이렉트 되는지 확인합니다.
- 로그인 실패시 `/login?error=true` 로 리다이렉트 되는지 확인합니다.
- 잘못된 자격 증명으로 로그인하면 오류 메시지가 표시되는지 확인합니다.


# 65. 사용자 정의 로그아웃 구성
**로그아웃 동작을 사용자 정의하는 방법**에 대해 설명하겠습니다.

## 로그아웃 구현의 필요성 ✔
- 로그아웃을 시도하면 **기본값으로 `login?logout GET` URL로 리다이렉션**됩니다. 최종 사용자는 단순히 로그인 페이지로 이동하게됩니다.
- 하지만 **로그아웃시 메시지를 남겨달라는 요구사항**이 들어온다면? 사용자 정의 로그아웃을 구현해야합니다.


## 사용자 logout 구현 ✔
1) `logout()` 메소드 구현
`logoutSuccessUrl("/login?logout=true")`를 설정하여 로그아웃 시 URL을 정의합니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	...
    http.logout(loc -> loc
	    .logoutSuccessUrl("/login?logout=true")  
    )
    ...
    return http.build();
```


2) 로그아웃 요청 구현
`displayLoginPage()` 메소드에 `@RequestParam("logout")`을 추가합니다. 로그아웃 요청이 들어오면 성공 메시지를 설정합니다.
```java
@RequestMapping(value = "/login", method = {RequestMethod.GET})  
public String displayLoginPage(@RequestParam(value = "error", required = false) String error,  
        @RequestParam(value = "logout", required = false) String logout, Model model) {  
    
    String errorMessge = null; 
     
    if(null != error) {  
        errorMessge = "Username or Password is incorrect !!";  
    }  
    
    if(null!= logout) {  
        errorMessge = "You have been successfully logged out !!";  
    }  
    
    model.addAttribute("errorMessge", errorMessge);  
    return "login.html";  
}
````


## 권장하는 추가 로그아웃 설정 ✔
- `invalidateHttpSession(true)`: **HTTP 세션을 무효화하여 모든 세션 정보를 삭제**합니다.
- `clearAuthentication()`: **보안 컨텍스트에서 인증 정보를 삭제**합니다.
- `deleteCookies("JSESSIONID")`: **JSESSIONID 쿠키를 삭제하여 사용자 정보를 완전히 제거**합니다.

```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	...
    http.logout(loc -> loc
	    .logoutSuccessUrl("/login?logout=true")  
        .invalidateHttpSession(true)  
        .clearAuthentication(true)  
        .deleteCookies("JSESSIONID")
    )
    ...
    return http.build();
```


## 사용자 logout 구현 테스트
- 로그인 후 로그아웃 버튼을 클릭하면 로그인 페이지에서 "성공적으로 로그아웃되었습니다." 메시지가 표시됩니다.


# 66. 스프링 시큐리티와 타임리프 통합
[Thymeleaf + Spring Security integration basics - Thymeleaf](https://www.thymeleaf.org/doc/articles/springsecurity.html)
Spring Security와 Thymeleaf를 통합하여 사용자 경험을 개선하는 방법에 대해 설명하겠습니다.

## 로그인 상태에 따른 UI 버튼 표시
- 현재 애플리케이션에서는 모든 사용자가 **로그인 여부와 관계없이 로그인, 로그아웃, 대시보드 버튼이 항상 표시**됩니다.
- 익명 사용자는 홈, 강좌, 연락처, 로그인 버튼만 표시하고, 로그인한 사용자는 대시보드와 로그아웃 버튼만 표시하도록 개선할 필요가 있습니다.

## Thymeleaf와 Spring Security 통합하는 이유 ✔
- Spring Security와 Thymeleaf의 통합을 통해 **인증 상태에 따라 UI 요소를 조절**할 수 있습니다.
- **`sec`라는 접두사를 사용**하여 보안 관련 메소드를 호출합니다.


## 설정 ✔
1) 의존성 추가
```xml
<dependency>  
    <groupId>org.thymeleaf.extras</groupId>  
    <artifactId>thymeleaf-extras-springsecurity6</artifactId>  
</dependency>
```

2) Spring Security Dialect 설정
Thymeleaf 템플릿 내에서 다음과 같이 추가 Dialect를 설정합니다.
```html
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity6">
````


## 구현 ✔
익명으로 접근 시 홈, 강좌, 연락처, 로그인 버튼만 표시되고, 로그인 후 대시보드와 로그아웃 버튼만 나타나게 구현합니다.
```html
<ul class="navbar-nav ms-auto my-2 my-lg-0 navbar-nav-scroll">  
    <li class="nav-item">  
        <a th:href="@{/home}" class="nav-link" aria-current="page" 
        sec:authorize="isAnonymous()">Home</a>  
    </li>    
    
    ....
       
    <li class="nav-item">  
        <a th:href="@{/logout}" class="nav-link" 
        sec:authorize="isAuthenticated()">Logout</a>  
    </li>
</ul>
```


## 테스트
- 익명으로 접근 시 홈, 강좌, 연락처, 로그인 버튼만 표시되고, 로그인 후 대시보드와 로그아웃 버튼만 표시되는지 확인합니다


# 67. SecurityContext 및 SecurityContextHolder의 역할
**인증(authentication)이 완료되면, 인증된 세부 정보를 SecurityContext 안에 저장**합니다. SecurityContext에 대해 기억해야 할 계층 구조는 다음과 같습니다.

## 인증과정
- 인증객체 생성(`Authentication`)합니다. 
- 인증이 완료되면, `SecurityContext` 객체 내부에 인증 세부 정보를 저장합니다
- `SecurityContext`는` SecurityContextHolder`라는 이름의 클래스에 의해 관리됩니다. 


## SecurityContextHolder 역할
- `SecurityContextHolder`는 **`SecurityContext` 객체를 저장하는 금고**와 같습니다. 
- **ThreadLocal이라는 전략을 사용**하여 `SecurityContext`를 저장할 것입니다. ([[쓰레드 로컬]])
![[Pasted image 20230328223636.png]]


## SecurityContextHolder 다른 전략들
▶ `MODE_THREADLOCAL`
- 설명 : **각 스레드가 자신의 보안 컨텍스트를 저장**합니다.
- **기본적으로 사용**되는 모드입니다. 각 요청은 개별 스레드에서 처리되므로, 요청 간에 보안 컨텍스트가 공유되지 않습니다.
- 스레드 안전성을 보장하며, 메모리 누수 문제를 피할 수 있습니다.

▶ `MODE_INHERITABLETHREADLOCAL`
- 설명: 비동기 메서드 호출 시, 부모 스레드의 보안 컨텍스트를 자식 스레드가 상속받습니다.
- `@Async` 어노테이션을 사용한 메서드에서 유용합니다. 비동기 작업을 수행할 때, 인증 정보를 자식 스레드가 활용할 수 있도록 합니다.
- 비동기 환경에서 보안 정보를 유지해야 할 필요가 있는 경우 유용합니다.

▶ `MODE_GLOBAL`
- 설명: 모든 스레드가 동일한 보안 컨텍스트 인스턴스를 공유합니다.
- 전역적으로 보안 정보를 공유해야 할 때 사용합니다. 모든 스레드가 같은 사용자 정보를 사용하므로, 멀티스레드 환경에서 데이터 충돌이 발생할 수 있습니다.
- 주의 깊게 사용해야 하며, 특정 요구 사항이 있는 경우에만 고려해야 합니다.
![[Spring_Security_70.png]]


## SecurityContext 역할 ✔
- 현재 인증된 사용자의 인증 정보와 권한 정보를 담고 있는 **Authentication을 보관하는 역할**입니다.

▶ 인증 객체 생성
- 주체(principal): 인증된 사용자의 이름
- 자격 증명(credentials): 비밀번호
- 권한(authorities): 최종 사용자의 권한
- 인증 성공 여부를 나타내는 boolean 변수 포함

![[Pasted image 20230328223636.png]]


## `SecurityContext` 인터페이스 이해
- `SecurityContext`는 인터페이스이며, 구현 클래스는 `SecurityContextImpl`입니다. 자체`SecurityContext`를 구현할 필요는 없습니다.

▶ 메소드
- `getAuthentication()`: **현재 인증된 사용자의 정보를 가져옵니다.**
- `setAuthentication(Authentication authentication)`: 인증 정보를 설정합니다.


# 68. 스프링 시큐리티에서 로그인 사용자 세부정보 로드하기
인증된 사용자가 누구인지 알아야 할 필요가 많습니다. 이번 강의에서는 비즈니스 로직이나 컨트롤러 클래스에서 **로그인 사용자 세부 정보를 로드하는 방법**을 알아보겠습니다.

## 두 방법의 장단점 ✔
▶ `SecurityContextHolder`
장점
- 간단하고 이해하기 **쉬운 코드 구조**.
- `SecurityContextHolder`를 통해 직접적으로 보안 컨텍스트를 접근하므로, **다양한 곳에서 인증 정보를 쉽게 가져**올 수 있습니다.

단점
- 정적 메서드를 사용하므로, **테스트하기 어려울 수** 있습니다. 특히, 단위 테스트 시 모킹(mocking)이 복잡해질 수 있습니다.
- **코드의 의존성이 높아져**, Spring의 DI(의존성 주입) 장점을 활용하지 못합니다.

▶ 메서드 인자로 `Authentication` 사용
장점
- 스프링의 DI를 활용하여 **코드가 더 깔끔하고 테스트하기 쉬워**집니다.
- **코드의 가독성**이 높아지고, 의도하는 바가 명확해집니다.

단점
- 메서드 인자로 **인증 정보를 전달해야 하므로, 약간의 오버헤드가 발생**할 수 있습니다.
- 메서드가 많은 경우, 인자에 **인증 정보를 지속적으로 추가해야 하므로 코드**가 길어질 수 있습니다.


## SecurityContextHolder 이용하는 방법 구현 ✔
```java
@GetMapping(value = "/username")
public String currentUserName() {
	Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
	String currentPrincipalName;
	
	if (null != authentication){
		String currentPrincipalName = authentication.getName();
	}
	
	return currentPrincipalName;
}
```


## Authentication객체 이용하는 방법 ✔
```java
@GetMapping(value = "/username")
public String currentUserName(Authentication authentication) {
	return authentication.getName();
}
```


## 테스트 
- 중단점을 설정하고 로그인 후 인증 객체를 확인해보자.
	- Authentication 유형: `UsernamePasswordAuthenticationToken`
	- 포함 정보: 사용자 이름, 권한, 원격 주소, 세션 ID, 인증 성공 여부 등

