[[Server] 토큰 기반 인증 VS 서버(세션) 기반 인증](https://mangkyu.tistory.com/55)
[🔐 JWT 완벽 가이드 (tistory.com)](https://jinn-blog.tistory.com/187)

---

# 99. 불투명 토큰 vs JSON 웹 토큰 (JWT)
## 토큰이란?
- 토큰은 사용자 **인증 시 생성**되는 고유 식별자(UUID) 또는 JSON Web Token(JWT) 형태일 수 있습니다. 사용자가 **로그인할 때 이러한 토큰이 생성**됩니다.


## 토큰의 역할
- 토큰은 **인증, 권한 부여, 세션 관리, 보안 강화 및 정보 전달** 등 여러 중요한 역할을 수행합니다. 이를 통해 시스템의 보안을 강화하고, 클라이언트와 서버 간의 신뢰를 구축하는 데 기여합니다.


## 토큰 두가지 형식
▶ Opaque 토큰
- **고유한 의미가 없는 무작위 문자열**. 예시) JSESSIONID 토큰, XSRF 토큰
- **백엔드 서버에서 세션을 유지 관리**하며, 각 요청마다 검증 필요.
- 브라우저 쿠키에 저장되며, UI 애플리케이션과 백엔드 간의 요청에 자동으로 첨부됨.
- 보안 API에 접근하기 위해 항상 백엔드 서버에 의존.

▶ JWT 토큰
- JSON 웹 토큰(JWT)은 사용자 관련 정보를 포함.
- Auth 서버에 의존하지 않고도 무상태로 직접 검증 가능.
- 클레임 정보와 만료 시간 등을 토큰에 포함.

![[Spring_Security_101.png]]


## 두 토큰은 언제 사용할까?
- Opaque 토큰: 보안 내부 네트워크와 같은 **중앙 검증이 가능한 시나리오**에 적합.
- JWT 토큰: 잦은 서버 호출 없이 빠른 검증이 필요한 **무상태(stateless) 분산 시스템**에 이상적.


# 100. 토큰 기반 인증의 장점
## opaque인증과정
![[Spring_Security_102.png]]

- 토큰 생성 과정
  1. 클라이언트가 로그인 페이지에 사용자 **이름과 비밀번호를 요청**.
  2. 백엔드 서버가 자격 증명을 검증하고, 올바르면 토큰을 응답으로 제공합니다. 현재 애플리케이션은 **Opaque 토큰을 생성하여 응답**합니다.
  3. 이후 클라이언트는 `/user/myAccount`와 같은 **요청을 할 때 이 토큰을 사용**합니다.


## jwt 토큰 장점
▶ **보안(Security)**
- 엔드 유저는 로그인 중에만 자격 증명을 공유합니다. 이후 요청에서는 토큰을 사용하여 자격 증명을 네트워크에 노출하지 않음.
- 해커가 토큰을 탈취한 경우, 백엔드 서버에서 모든 토큰을 무효화할 수 있음.

▶ **만료(Expiration)**
- 백엔드 서버는 특정 만료 시간을 설정할 수 있으며, 토큰이 만료되면 자동으로 무효화됨.

▶ **자가 수용적(Self-contained)**
- JWT 토큰은 사용자 정보와 권한을 포함하여, 백엔드 서버에 의존하지 않고 직접 정보를 읽을 수 있음.

▶ **재사용성(Reusability)**
- 여러 애플리케이션에서 동일한 토큰을 사용 가능합니다. 예를 들어, Google 계정으로 여러 서비스(지메일, 구글 포토 등)에 로그인할 때, 한 번의 로그인으로 모든 서비스에 접근.

▶ **플랫폼 간 호환성 (Cross-Platform Compatibility)**
- 동일한 토큰이 웹, 모바일, IoT 등 다양한 플랫폼에서 사용 가능.

▶ **무상태성(Statelessness)**
- 토큰은 세션 상태를 유지할 필요 없이 사용자 정보를 담고 있어, 로드 밸런서를 사용할 때도 유연하게 서버 간에 요청을 분산할 수 있습니다.
- 이는 마이크로서비스 아키텍처에 필수적이며, 여러 인스턴스에서 요청을 처리할 수 있도록 함.


# 101. JWT 토큰에 대한 심층 탐구

## 개요
- 현재 Spring Boot 웹 애플리케이션은 Opaque 토큰 형식을 사용하며, 이를 JWT 토큰 형식으로 개선하고자 함.
- JWT 토큰의 형식과 생성 방법을 이해하는 데 중점을 두는 강의.


## JWT 토큰이란? ✔
- JSON 형식을 사용하여 **웹 요청에 사용되도록 설계된 토큰**입니다. 
- JWT 토큰은 인증 및 권한 부여 시나리오에서 사용자 관련 데이터를 공유할 수 있습니다. 이를 통해 클라이언트와 서버 간의 **세션 유지 부담을 줄일 수** 있습니다.


## JWT 토큰 구성 ✔
![[Pasted image 20241008082148.png]]
모든 JWT 토큰은 세 부분으로 구성되며 마침표로 구분됩니다.
```
HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret )
```

▶ 헤더
- 토큰과 관련된 메타데이터 정보를 포함합니다. ex) **서명에서 사용하는 알고리즘**

▶ 내용(Payload)
- 사용자 관련 정보 및 역할, 권한 등의 **세부 정보를 포함**합니다. 
- 비즈니스 로직에 필요한 모든 정보를 포함할 수 있으나, **비밀번호는 저장하지 않아야** 함.

▶ 서명
- JWT의 마지막 부분은 디지털 서명입니다. 이 부분은 선택적이며, 신뢰할 수 있는 당사자와 공유할 때 필요합니다.
- 서명은 토큰이 생성될 때 초기화되며, 이를 통해 네트워크에서 **데이터가 변조되지 않았음을 확인**할 수 있습니다.
- **비밀 값은 백엔드에서 신중하게 관리**되어야 하며, 노출되면 큰 보안 문제가 발생할 수 있습니다. 비밀 값은 환경 변수 또는 비밀 금고에 저장하여 안전하게 보호해야합니다.


## JWT의 작동 원리 ✔
![[Pasted image 20241008084306.png]]
- **백엔드 서버에서 JWT를 생성해주고, 클라이언트에서는 생성한 JWT를 저장해서 요청에 이용합니다.** 클라이언트 입장에서는 JWT내용을 알필요가 없습니다.




## 변조 감지 과정 ✔
![[Spring_Security_107 1.png]]
- JWT 토큰이 생성될 때, **백엔드 서버는 해싱 알고리즘을 사용하여 서명을 생성**합니다.
- 요청 시 토큰을 수신하면, 서버는 동일한 계산을 수행하여 **서명 값을 비교**하고 변조가 발생하면 서명 값이 일치하지 않아 403 에러를 발생시킴.


# 102. JWT 토큰을 사용하기 위한 프로젝트 구성하기
## 의존성 추가
- JWT 토큰 생성 및 검증은 복잡할 수 있지만, 특정 라이브러리를 사용하면 쉽게 처리 가능합니다.
```xml
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-api</artifactId>  
    <version>0.12.5</version>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-impl</artifactId>  
    <version>0.12.5</version>  
    <scope>runtime</scope>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-jackson</artifactId>  
    <version>0.12.5</version>  
    <scope>runtime</scope>  
</dependency>
```


## JWT SecurityConfig로 변경하기
1) 무상태 만들기
JSESSIONID 비활성화하기 위해 `SessionCreationPolicy.STATELESS`로 변경
   
2) `securityContext()` 제거
   - 더 이상 필요하지 않으므로 관련 구성을 제거

3) CORS 설정
   - 클라이언트 -> 서버로 오는 모든 헤더를 수락으로 변경
   - 백엔드에서 헤더를 보내기 위해 `setExposedHeaders(Authorization)` 추가함.

```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {   
    http.sessionManagement(sessionConfig -> sessionConfig.sessionCreationPolicy(SessionCreationPolicy.STATELESS))  
            .cors(corsConfig -> corsConfig.configurationSource(new CorsConfigurationSource() {  
                @Override  
                public CorsConfiguration getCorsConfiguration(HttpServletRequest request) {  
                    CorsConfiguration config = new CorsConfiguration();  
                    config.setAllowedOrigins(Collections.singletonList("http://localhost:4200"));  
                    config.setAllowedMethods(Collections.singletonList("*"));  
                    config.setAllowCredentials(true);  
                    config.setAllowedHeaders(Collections.singletonList("*"));  
                    config.setExposedHeaders(Arrays.asList("Authorization"));  
                    config.setMaxAge(3600L);  
                    return config;  
                }  
            }))

	...

	retrun http.build()
}
```


## JWT 토큰 생성 구조
1) 필터를 사용하여 토큰 생성
```java
public class JWTTokenGeneratorFilter extends OncePerRequestFilter {  
  
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {  
		//JWT 생성 로직 작성
	}  
	
	@Override  
	protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {  
		return !request.getServletPath().equals("/user");  //!있음
	}  
}
```
- `extends OncePerRequestFilter` : 여러 번 JWT 토큰을 생성하고 싶지 않기 때문에
- `doFilterInternal()` : JWT 생성 로직 작성
- `shouldNotFilter()` : 실행조건을 로그인 경로(`/user`)에서만 필터가 실행되도록 설정.

2) config의 추가하기
```java
http.addFilterAfter(new JWTTokenGeneratorFilter(), BasicAuthenticationFilter.class)
```
- **인증 성공 후 JWT 토큰을 생성**하는 옵션입니다. 여기서 인증을 진행하진 않습니다.


## JWT 토큰 유효성 검사 구조
1) 필터를 사용하여 유효성 검사 토큰 생성
```java
public class JWTTokenValidatorFilter extends OncePerRequestFilter {  

    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {  
       // JWT 유효성 검사 로직 작성
    }  
  
    @Override  
    protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {  
        return request.getServletPath().equals("/user");  //!없음
    }  
}
```
- `extends OncePerRequestFilter` : 여러 번 JWT 토큰을 생성하고 싶지 않기 때문에
- `doFilterInternal()` : JWT 유효성 검사 로직 작성
- `shouldNotFilter()` : 실행조건을 로그인 경로(`/user`)가 아닐때만 필터가 실행되도록 설정.

2) config의 추가하기
```java
http.addFilterAfter(new JWTTokenGeneratorFilter(), BasicAuthenticationFilter.class)
	.addFilterBefore(new JWTTokenValidatorFilter(), BasicAuthenticationFilter.class)
```
- JWT 토큰이 유효하다면, 성공했음을 알려서 다시 인증을 시도하지 않도록 할 것입니다


# 103. JWT 토큰을 생성하는 로직 구축하기

## JWT 생성 비즈니스 로직 작성
1) 인증된 사용자만 jwt 토큰 생성 가능
2) 시큐리티key 가져와서 비밀키 객체 생성하기
3) JWT 만들기

```java
@Override  
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,  
        FilterChain filterChain) throws ServletException, IOException {  

	//인증된 사용자 추출하기
    Authentication authentication = SecurityContextHolder.getContext().getAuthentication();  
    if (null != authentication) {  
		// 시큐리티 가져와 비밀키 생성
        Environment env = getEnvironment();  
        if (null != env) {  
            String secret = env.getProperty(ApplicationConstants.JWT_SECRET_KEY,  
                    ApplicationConstants.JWT_SECRET_DEFAULT_VALUE);  
            SecretKey secretKey = Keys.hmacShaKeyFor(secret.getBytes(StandardCharsets.UTF_8));  

			//jwt 생성
            String jwt = Jwts.builder().issuer("Eazy Bank").subject("JWT Token")  
                    .claim("username", authentication.getName())  
                    .claim("authorities", authentication.getAuthorities().stream().map(  
                            GrantedAuthority::getAuthority).collect(Collectors.joining(",")))  
                    .issuedAt(new Date())  
                    .expiration(new Date((new Date()).getTime() + 30000000))  
                    .signWith(secretKey).compact();  

			// Authentication 객체
            response.setHeader(ApplicationConstants.JWT_HEADER, jwt);  
        }  
    }  
    filterChain.doFilter(request, response);  
}

// API가 "/user"일때만 실행하는 필터 만들기
@Override  
protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {  
    return !request.getServletPath().equals("/user");  
}
```


## JWT 생성 정보
JWT 생성할때 사용하는 속성에 대해서 알아보자

- `Jwts.builder()` : 사용하여 JWT를 생성합니다.
- `issuer` : 토큰 발급자 정보를 설정합니다.
- `subject` : 토큰의 주제(주로 사용자 정보를 나타냄)를 설정합니다.
- `claim` : 사용자 이름과 권한 정보를 클레임으로 추가합니다. 권한은 스트림을 사용해 콤마로 구분된 문자열로 변환합니다.
- `issuedAt` : 토큰 발급 시간을 현재 시간으로 설정합니다.
- `expiration` : 토큰의 만료 시간을 현재 시간으로부터 30,000,000 밀리초(약 8시간) 후로 설정합니다.
- `signWith` : 비밀 키를 사용하여 JWT를 서명합니다.
- `compact()` : 모든 설정을 마친 후, JWT 문자열을 생성합니다.


## 환경변수 클래스 만들기
JWT 관련 설정에 필요한 상수 값을 정의하여 유지 보수를 용이하게 합니다. 이러한 상수는 애플리케이션 전반에서 일관되게 사용되며, 환경 설정 파일과의 연결을 통해 보안성을 유지합니다.
```java
public final class ApplicationConstants {  
    public static final String JWT_SECRET_KEY = "JWT_SECRET";  
    public static final String JWT_SECRET_DEFAULT_VALUE = "jxgEQeXHuPq8VdbyYFNkANdudQ53YUn4";  
    public static final String JWT_HEADER = "Authorization";  
}
```


# 104. JWT 토큰을 검증하는 로직 구축하기
## JWT 검증 비즈니스 로직 작성
1) header에서 jwt 가져오기
2) 시큐리티key를 이용해서 비밀키 만들기
3) 비밀키를 활용해서 검증하기
4) 검증이 완료되면, Payload내용으로 토큰 만들기
5) `SecurityContext`에 저장해, 이후의 요청 처리 과정에서 사용

```java
@Override  
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)  
        throws ServletException, IOException {  

   // header에서 jwt가져오기
   String jwt = request.getHeader(ApplicationConstants.JWT_HEADER);  
   
   if(null != jwt) {  
       try {  
	       //시큐리티key를 이용해서 비밀키 만들기
           Environment env = getEnvironment();  
           if (null != env) {  
               String secret = env.getProperty(ApplicationConstants.JWT_SECRET_KEY,  
                       ApplicationConstants.JWT_SECRET_DEFAULT_VALUE);  
               SecretKey secretKey = Keys.hmacShaKeyFor(secret.getBytes(StandardCharsets.UTF_8));  
               
               if(null !=secretKey) {  
	               //비밀키를 활용해서 검증하기
                   Claims claims = Jwts.parser().verifyWith(secretKey)  
                            .build().parseSignedClaims(jwt).getPayload();  

				   //Payload내용으로 토큰 만들기
                   String username = String.valueOf(claims.get("username"));  
                   String authorities = String.valueOf(claims.get("authorities"));  
                   List<GrantedAuthority> authorityList = AuthorityUtils.commaSeparatedStringToAuthorityList(authorities);  
				   Authentication authentication = new UsernamePasswordAuthenticationToken(username, null, authorityList);

				   // 인증된 사용자 저장하기
                   SecurityContextHolder.getContext().setAuthentication(authentication);  
               }  
           }  
  
       } catch (Exception exception) {  
           throw new BadCredentialsException("Invalid Token received!");  
       }  
   }  
    filterChain.doFilter(request,response);  
}  

//API가 "/user"가 아닐때 실행하기
@Override  
protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {  
    return request.getServletPath().equals("/user");  
}
```


## 검증 코드 살펴보기
JWT 검증 및 클레임 추출
```java
Claims claims = Jwts.parser().verifyWith(secretKey).build().parseSignedClaims(jwt).getPayload();
```
- JWT를 파싱하고 서명을 검증하여 클레임을 추출합니다. 이 과정에서 비밀 키를 사용하여 JWT의 유효성을 확인합니다.
- `Claims`는 JWT의 페이로드에 포함된 정보를 담고 있는 객체입니다.


# 105. JWT 토큰 기반 인증을 위한 클라이언트 측 변경 사항 적용하기

## 로그인 컴포넌트 수정 ✔
- 파일: `login.component.ts`
- 변경 사항:
  - 초기 로그인 작업 성공 후, 응답에서 "Authorization" 헤더를 읽어 `sessionStorage`에 저장.
  - JWT 토큰 값은 "Authorization" 키 이름으로 저장하여 애플리케이션의 다른 파일에서 참조할 수 있도록 설정.
  - `sessionStorage` 사용 이유: 사용자가 탭이나 브라우저를 닫을 때마다 데이터가 삭제됨.

## 인터셉터 파일 수정 ✔
- 파일: `app.request.interceptor.ts`
- 변경 사항:
  - 요청을 가로채어 각 요청마다 JWT 토큰을 전송.
  - 로그인 작업 시에는 사용자 자격 증명을 HttpBasic 형식으로 전송하고, 다른 모든 요청에서는 `sessionStorage`에서 JWT 토큰을 읽어 "Authorization" 헤더에 설정.

## 로그아웃 컴포넌트 수정 ✔
- 파일: `logout.component.ts`
- 변경 사항:
  - 사용자가 로그아웃 버튼을 클릭할 때, 권한 관련 정보를 `sessionStorage`에서 삭제.


# 106. 애플리케이션을 실행하여 JWT 변경 사항 검증하기

## JWT 생성 테스트
  - `JWTTokenGeneratorFilter`에 중단점 설정하고 `/user` API를 호출하여 사용자를 인증해보자
   - 로그인 후, 쿠키와 Authorization 헤더에서 JWT 토큰 확인.
  - JWT 토큰을 복사하여 [jwt.io](https://jwt.io) 웹사이트에 붙여넣어 내용 검증.


## JWT 검증 사용
  - `JWTTokenValidatorFilter`에 중단점 설정하고 `myAccount` API를 호출하여 Authorization 헤더에 JWT 토큰 추가해보자
  - JWT 토큰이 유효하면 사용자 이름과 권한을 가져와 Authentication 객체 생성후 응답


## 변조된 토큰 테스트
  - JWT 토큰의 값을 조작하여 요청을 보내면 SignatureException 발생.
- 500 내부 서버 오류가 발생하지만, 예외처리해서 400 또는 401 오류가 발생해야 함


# 107. JWT 토큰 만료 시나리오 검증하기


## 만료 테스트
- JWT 토큰의 만료 시간을 3000밀리초(3초)로 줄이고, `JWTTokenValidator`의 catch 블록에 중단점 설정.
- 사용자 이름과 자격 증명으로 `user` API 호출하고 응답에서 Authorization 헤더의 JWT 토큰 값을 복사.
- 복사한 JWT 토큰 값을 사용하여 `myAccount` API 호출하면 `ExpiredJwtException`: JWT 토큰이 만료되었다는 상세 메시지 출력.

- 생성할 모든 JWT 토큰은 특정 만료 시간을 가지며, 현재 `JWTTokenGeneratorFilter`에서 만료 시간을 약 8시간으로 설정.
- 이번 강의에서는 만료 시간 이후에 JWT 토큰에 액세스하려고 하는 시나리오를 데모로 보여줌.


## 추가 예외처리
- 현재 catch 블록에서 항상 `BadCredentialsException`을 던지고 있는 문제점. 여러 개의 catch 블록을 작성하여 다양한 유형의 예외를 처리해보자
- 예를 들어, 만료 시나리오와 변조 시나리오를 별도로 처리.


# 108. 사용자 정의 또는 수동 인증을 위한 AuthenticationManager 게시하기 - 1부
## Controller 구현
 - RequestBody 또는 RequestHeaders 내부에 자격 증명을 받아야 하는 요구 사항입니다.
 
```java
@RestController  
@RequiredArgsConstructor  
public class UserController {

	private final CustomerRepository customerRepository;  
	private final PasswordEncoder passwordEncoder;  
	private final AuthenticationManager authenticationManager;  
	private final Environment env;

	...

	@PostMapping("/apiLogin")  
	public ResponseEntity<LoginResponseDTO> apiLogin (@RequestBody LoginRequestDTO loginRequest) {  
	    String jwt = "";  
	    // 인증 객체 생성  
	    Authentication authentication = UsernamePasswordAuthenticationToken.unauthenticated(loginRequest.username(), loginRequest.password());  
	    // 인증 수행  
	    Authentication authenticationResponse = authenticationManager.authenticate(authentication);  
	  
	    if(null != authenticationResponse && authenticationResponse.isAuthenticated()) { // 인증확인  
	        if (null != env) {  
	            // 환경변수 가져와서 시크릿키 생성하기  
	            String secret = env.getProperty(ApplicationConstants.JWT_SECRET_KEY, ApplicationConstants.JWT_SECRET_DEFAULT_VALUE);  
	            SecretKey secretKey = Keys.hmacShaKeyFor(secret.getBytes(StandardCharsets.UTF_8));  
	              
	            // jwt 생성  
	            jwt = Jwts.builder().issuer("Eazy Bank").subject("JWT Token")  
	                    .claim("username", authenticationResponse.getName())  
	                    .claim("authorities", authenticationResponse.getAuthorities().stream().map(  
	                            GrantedAuthority::getAuthority).collect(Collectors.joining(",")))  
	                    .issuedAt(new java.util.Date())  
	                    .expiration(new java.util.Date((new java.util.Date()).getTime() + 30000000))  
	                    .signWith(secretKey).compact();  
	        }  
	    }  
	    // 응답 반환하기(확인용으로 header, body 둘다 포함)  
	    return ResponseEntity.status(HttpStatus.OK).header(ApplicationConstants.JWT_HEADER,jwt)  
	            .body(new LoginResponseDTO(HttpStatus.OK.getReasonPhrase(), jwt));  
	}
}
```


## Config 설정
1) 인증 프로세스 추가
인증 프로세스를 수동으로 시작하려면 먼저 Bean을 생성해야 합니다
```java
@Configuration  
@Profile("!prod")  
public class ProjectSecurityConfig {
	...

	@Bean  
	public AuthenticationManager authenticationManager(UserDetailsService userDetailsService, PasswordEncoder passwordEncoder) {  
	    EazyBankUsernamePwdAuthenticationProvider authenticationProvider = new EazyBankUsernamePwdAuthenticationProvider(userDetailsService, passwordEncoder);  
	    ProviderManager providerManager = new ProviderManager(authenticationProvider); 
	    // Authentication 객체 내부의 비밀번호를 지우지 않을 것입니다
	    providerManager.setEraseCredentialsAfterAuthentication(false);  
	    return providerManager;  
	}
}
```

2) 보안설정 수정
CSRF 예외 리스트에 추가와 API 허용
```java
@Bean  
SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
	...
	
	http.csrf(csrfConfig -> csrfConfig.csrfTokenRequestHandler(csrfTokenRequestAttributeHandler)  
		//CSRF 예외 리스트에 추가
        .ignoringRequestMatchers( "/contact","/register", "/apiLogin") 
        .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse()))
    http.authorizeHttpRequests((requests) -> requests  
	    // "/apiLogin" 허용
        .requestMatchers("/notices", "/contact", "/error", "/register", "/invalidSession", "/apiLogin").permitAll());

	retrun http.build()
}
```


# 109. 사용자 정의 또는 수동 인증을 위한 AuthenticationManager 게시하기 - 2부

## 테스트
- 요청
![[Pasted image 20241008231808.png]]

- 응답
