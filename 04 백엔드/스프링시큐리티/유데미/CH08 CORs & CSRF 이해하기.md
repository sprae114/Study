# 요약
- CORS에 대해서 이해할 수 있음(p.76)
- CORS의 해결 코드를 구현할 수 있음(p.76)
- 

# 76. CORs 소개

## CORS 오류 발생
- CORS 정책에 의해 차단 : 웹서버 (`localhost:4200`) 에서 백엔드(`localhost:8080/notices`)로의 연결 실패합니다. 


# 77. CORs 문제를 해결하기 위한 가능한 옵션들
[[10분 테코톡] 성하의 CORS (youtube.com)](https://www.youtube.com/watch?v=4iXWkBi2hCA)

 CORS(교차 출처 리소스 공유)에 대해 설명하겠습니다.

## 실제 사례: 전화 통화
- 휴대전화 예시
  - 한국에 거주하는 경우, **같은 +82 국가 코드를 가진 전화를 쉽게 받을 수 있음**.
  - 반면, **다른 +11 국가 코드를 가진 전화를 받으면 경계**하게 됨.

- 친구와 가족의 전화
  - 저장된 연락처를 통해 전화를 받으면 별다른 걱정 없이 받을 수 있음.
  - 하지만 모르는 번호의 전화를 받을 경우, 사기 전화일 수 있다고 생각하고 조심하게 됨.


## CORS란?
- CORS는 브라우저 클라이언트에서 실행되는 **스크립트가 다른 출처의 리소스와 상호작용할 수 있도록 하는 프로토콜**입니다.

## 출처(Origins)란?  ✔
URL의 조합으로, **프로토콜(HTTP/HTTPS), 도메인 이름, 포트 번호로 구성**됩니다.

- `https://www.google.com` (하나의 출처)
- `https://www.amazon.com` (다른 출처)
두 출처의 **도메인 이름이 다르기 때문에 서로 다른 출처로 간주**됩니다. (포트가 달라도 마찬가지 입니다.)


## CORS 정책이란? ✔
- 기본적으로 브라우저는 **서로 다른 출처 간의 통신을 차단**합니다.  예를 들어, `domain1.com`의 UI 애플리케이션이 `domain2.com`의 API를 호출하려고 할 때, CORS 정책에 의해 차단됩니다.
![[Pasted image 20241002071235.png]]

- 이러한 정책은 일반적으로 **보안 위협을 방지하기 위한 예방 조치**입니다. 서로 다른 출처의 애플리케이션 간의 통신은 기본적으로 차단되어 해커의 공격을 방지합니다.


## Preflight 요청 흐름
1. 브라우저가 **두 출처 간의 통신을 감지하면 preflight 요청을 수행**
2. 백엔드 서버는 허용된 출처의 세부 정보를 포함한 응답 헤더를 보내면 성공, 이러한 정보를 제공하지 않으면 실패


# 78. 스프링 시큐리티를 사용하여 CORs 문제 해결하기
현재 브라우저의 CORS 정책에 따르면 서로 다른 두 애플리케이션은 통신할 수 없습니다. 이러한 경우, 브라우저에 **두 출처 간의 통신을 허용하도록 알리는 방법**을 살펴보겠습니다.

## 접근 방식 1: @CrossOrigin 주석 사용
- 웹 애플리케이션이 **다른 출처의 리소스에 접근할 수 있도록 허용하는 어노테이션**입니다.
- 장점 : 어노테이션으로 **간편한 설정 및 세밀한 제어**가 가능합니다.
- 단점 : 프로젝트 내에 여러 컨트롤러가 있는 경우, **각 컨트롤러에 주석을 반복적으로 추가해야 함**. 새로운 출처 추가 시 모든 컨트롤러를 업데이트해야 하는 번거로움이 있음

▶ 기본 사용법
`@CrossOrigin` 어노테이션은 주로 컨트롤러 클래스나 메서드에 적용되어 CORS 설정을 지정합니다
```java
@CrossOrigin(origins = "http://example.com")
@RestController
public class MyController {
    @GetMapping("/data")
    public ResponseEntity<String> getData() {
        return ResponseEntity.ok("Hello from server!");
    }
}
```


## 접근 방식 2: Spring Security 사용 ✔
- Spring Security를 사용하여 **CORS를 모든 컨트롤러에 대해 전역적으로 설정**할 수 있습니다.

```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
    http.cors(corsConfig -> corsConfig.configurationSource(new CorsConfigurationSource() {  
                @Override  
                public CorsConfiguration getCorsConfiguration(HttpServletRequest request) {  
                    CorsConfiguration config = new CorsConfiguration();  
                    config.setAllowedOrigins(Collections.singletonList("https://localhost:4200"));  
                    config.setAllowedMethods(Collections.singletonList("*"));  
                    config.setAllowCredentials(true);  
                    config.setAllowedHeaders(Collections.singletonList("*"));  
                    config.setMaxAge(3600L);  
                    return config;  
                }  
            }))
...
```
- `setAllowedOrigins()`: 수락할 출처 목록 제공
- `setAllowedMethods()`: 허용할 HTTP 메소드 설정
- `setAllowCredentials()`: 자격 증명이나 쿠키 전송 허용
- `setAllowedHeaders()`: 수락할 헤더 목록 정의
- `setMaxAge()`: CORS 설정을 기억할 시간(예: 3600초)


# 79. 스프링 시큐리티 내의 기본 CSRF 보호 데모
이번 강의에서는 CSRF(Cross-Site Request Forgery) 보안 공격에 대해 집중하겠습니다.

## CSRF 공격의 개념 ✔
- **위조의 의미**: 누군가가 여러분을 이용하거나 동의 없이 행동하는 것을 위조라고 부릅니다. 예를 들어, 누군가가 여러분의 수표책을 가지고 서명을 위조해 은행 계좌에서 자금을 인출하려고 한다면, 이는 위조로 간주됩니다.
- **CSRF 공격 방식**: 해커는 사용자의 **계정 정보(사용자 이름, 비밀번호, 쿠키 등)를 훔치지 않고, 사용자를 속여 그들의 의지와 상관없이 행동을 취하게 합니다.**


## Spring Security의 CSRF 보호 
다음과 같이 요청할때, CSRF 설정에 따른 응답을 보면
![[Pasted image 20241002104610.png]]

1. CSRF 관련 설정을 비활성화하면??
   - 현재 보안 설정에서 Spring Security의 CSRF 보호가 비활성화되어 있습니다. 이로 인해 `register` API와 같은 POST 요청이 가능해집니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	http. ...
		.csrf(csrfConfig -> csrfConfig.disable())
	
	return http.build();
}
```


2. CSRF 보호 활성화되면?
   - CSRF 보호를 비활성화하면 Spring Security는 **모든 post, put, delete 관련 메소드를 보호**합니다.
   - POST API를 호출하면 **403 에러가 발생**합니다. 이는 Spring Security가 세션을 무효화하려고 시도하기 때문입니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {  
	http. ...
		// .csrf(csrfConfig -> csrfConfig.disable())
	
	return http.build();
}
```


3. CSRF 토큰 오류
   - 세션을 무효화한 후 POST 요청을 보내면 `'잘못된 CSRF 토큰 'null'이 요청 매개변수에서 발견되었습니다'`라는 메시지와 함께 403 에러가 발생합니다.
   - CSRF 토큰이 누락되면 Spring Security는 공격을 의심하고 요청을 차단합니다.


## UI 애플리케이션에서의 동작
- GET 요청: 현재 UI 애플리케이션에서 `notices` API를 호출할 수 있습니다. GET 메소드에는 CSRF 검사가 적용되지 않습니다.
- POST 요청: CONTACT US 페이지에서 정보를 입력하고 메시지를 보내면, POST 요청이 발생하고 Spring Security가 요청을 차단하여 웹 콘솔에서 403 Forbidden 에러가 발생합니다.



# 80. CSRF 공격 소개
## CSRF 공격 시나리오
예시: 넷플릭스와 악성 웹사이트

1. **사용자 로그인**: 사용자가 넷플릭스에 로그인하고 영화나 웹 시리즈를 시청합니다. 이 과정에서 넷플릭스는 쿠키를 생성하여 브라우저에 저장합니다.
   
2. **악성 웹사이트 방문**: 사용자가 브라우저 탭을 열고 `evil.com`이라는 악성 웹사이트에 접근합니다. 이 웹사이트는 해커가 운영합니다.

3. **유인 링크 클릭**: 악성 웹사이트에 있는 링크나 배너를 클릭하게 되면, 숨겨진 폼이 제출됩니다. 이 폼은 넷플릭스의 `changeEmail` 경로로 POST 요청을 보냅니다.

4. **쿠키 첨부**: 브라우저는 요청에 넷플릭스의 쿠키를 첨부하여 요청을 보냅니다. 이 요청에는 해커의 이메일 주소가 포함되어 있습니다.

## CSRF 포인트
- CSRF 접근 방식도 **JSESSIONID 쿠키나 로그인 관련 쿠키를 요청의 첨부**하고 있습니다.
- 넷플릭스 백엔드는 **요청이 악의적인 웹사이트에서 오는지 여부를 구분할 수 없습**니다. **요청이 동일한 도메인에서 발생**하기 때문입니다.
- CORS 정책이 적용되더라도, 이 시나리오에서는 CSRF 공격이 여전히 가능하다는 점을 강조하고 싶습니다. 브라우저는 요청이 해당 도메인에서 발생한 것으로 인식합니다.


# 81. CSRF 공격을 처리하기 위한 해결책
[[Spring Security] CORS, CSRF란? — 천천히 꾸준하게 (tistory.com)](https://jaykaybaek.tistory.com/29#2.%20CSRF%EB%9E%80%3F-1)

## CSRF 토큰 사용
CSRF 공격을 방어하기 위해 백엔드 서버는 **CSRF 토큰을 사용하여 요청의 출처를 구별할 수** 있어야 합니다. 

CSRF 토큰은 다음과 같은 특징을 가집니다:
- **랜덤 토큰 값**: CSRF 토큰은 사용자 세션마다 고유해야 하며, 해커가 추측하기 어려운 무작위 값이어야 합니다.
- **안전한 길이**: CSRF 토큰은 최소한 이중 길이의 랜덤 문자열이어야 합니다.


## CSRF 토큰은 어떻게 요청의 출처를 구별할까?
- 해커의 자바스크립트가 실행되는 hacker.com이란 도메인은 이러한 쿠키가 저장되지 않았습니다. 로그인할 때 **쿠키에 저장된 CSRF 토큰은 neflex.com이란 도메인에 저장**되었습니다. 
- 이러한 제약 때문에 해커가 아무리 hacker.com 도메인에서 자바스크립트를 실행해도 넷플릭스의 백엔드 서버에서는 CSRF 쿠키의 토큰이 있어야 로직을 수행하기 때문에 동작하지 않습니다.


# 82. 백엔드 애플리케이션 내에서 CSRF 토큰 솔루션 구현하기 - 1부
 CSRF 토큰 관련 변경 사항을 구현하기 전에 Spring Security 라이브러리 내의 중요한 클래스와 인터페이스에 대해 설명하겠습니다.

## CSRFToken 인터페이스
- `CSRFToken` 인터페이스는 **CSRF 토큰이 어떻게 생겨야 하는지에 대한 정보를 제공**합니다.
- Spring Security는 `CsrfToken`의 구현인 `DefaultCsrfToken`을 기본적으로 사용합니다

▶ 주요 메소드
- `getHeaderName()`
- `getParameterName()`
- `getToken()`


## CsrfTokenRepository 인터페이스
- 개발자가 로그인 작업 중에 **CSRF 토큰을 생성을 구현할 필요 없습**니다. Spring Security 프레임워크가 `CsrfTokenRepository` 인터페이스를 통해 처리합니다.

▶ 구현체
- `HttpSessionCsrfTokenRepository` : CSRF 토큰 세부 정보가 HTTP 세션 내에 저장됩니다. 권장되지 않음
- `CookieCsrfTokenRepository` : CSRF 토큰을 **쿠키 내에 저장하는 방법**을 제공합니다.


## CsrfFilter
- 후속 요청에서 POST, PUT 또는 DELETE 유형인 경우, **CSRF 토큰을 검증하는 과정을 처리**합니다. 유효하지 않은 경우  `AccessDeniedException` 예외를 발생시킵니다.


## 로그인 작업 후 CSRF 토큰을 생성 구현
1) config 설정
**쿠키 내에 CSRF 토큰을 생성 후 넣어야**합니다.
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
	http.securityContext(contextConfig -> contextConfig.requireExplicitSave(false))
			.csrf(csrfConfig -> csrfConfig.csrfTokenRepository(
					CookieCsrfTokenRepository.withHttpOnlyFalse())
		        .ignoringRequestMatchers( "/contact","/register")  
    )
}

```
- `withHttpOnlyFalse()` 메소드를 호출하여 쿠키의 `httpOnly` 속성을 `false`로 설정합니다. `HttpOnly` 속성이 `false`로 설정되면, **JavaScript 코드에서 쿠키에 접근할 수 있어 CSRF 토큰을 쉽게 읽고 사용할 수** 있습니다.


2) 필터 설정
CSRF 토큰 값을 생성할 때, 백그라운드에서 토큰이 느리게 생성됩니다. 그래서 **토큰을 수동으로 읽는 로직을 작성**해야 합니다.
```java
public class CsrfCookieFilter extends OncePerRequestFilter {  
  
  
    @Override  
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)  
            throws ServletException, IOException {  
        CsrfToken csrfToken = (CsrfToken) request.getAttribute(CsrfToken.class.getName());  
        // Render the token value to a cookie by causing the deferred token to be loaded  
        csrfToken.getToken();  
        filterChain.doFilter(request, response);  
    }  
}
```
- CSRF 토큰 객체를 `CsrfToken`으로 형변환하고, `getToken()` 메소드를 호출하여 실제 토큰을 생성합니다.



3) 필터 등록하기
기본 인증 필터 실행 후에 이 필터가 실행되도록 설정합니다
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
	http.securityContext(contextConfig -> contextConfig.requireExplicitSave(false))
			.csrf(csrfConfig -> csrfConfig.csrfTokenRepository(
					CookieCsrfTokenRepository.withHttpOnlyFalse()))
			.addFilterAfter(new CsrfCookieFilter(), BasicAuthenticationFilter.class)
}
```

----
# 미완성⛏
# 83. 백엔드 애플리케이션 내에서 CSRF 토큰 솔루션 구현하기 - 2부

## CSRF 토큰의 후속 검증을 처리 추가 구현
위의 로그인 작업 후 CSRF 토큰을 생성 구현 진행 후 계속



# 84. UI 애플리케이션 내에서 CSRF 토큰 솔루션 구현하기



# 85. 공용 API에 대한 CSRF 보호 무시하기