# 요약
- 인증과 인가의 차이를 알 수 있음(p.86)
- 스프링에서 단일 권한 구현 방법(p.87)
- 스프링에서 복수 권한 구현 방법(p.88~89)
- 권한에 따른 API 보호 설정(p.90)
- 권한과 역할의 차이(p.91~92)
- 인가 이벤트 구현 방법(p.93)


# 86. 인증 (Authentication) vs 인가 (Authorization)

## 인증(Authentication) VS 권한 부여(Authorization) ✔
▶ 인증 (Authentication)
- 사용자의 **신원을 확인**하는 과정입니다. 시스템에 접근할 수 있는 권한이 있는지를 판별합니다.
- 인증은 **권한 부여 전에 수행**됩니다.
- 일반적으로 사용자의 **로그인 정보를 필요**로 합니다.
- 인증에 실패할 경우, 일반적으로 **401 오류 응답**을 받게 됩니다.
- 예시: 은행의 고객이나 직원이 애플리케이션에서 작업을 수행하기 위해서는 먼저 자신의 신원을 증명해야 합니다.

 ▶권한 부여 (Authorization)
- 사용자의 **권한이나 역할을 확인**하여 자원에 접근할 수 있는지를 결정합니다.
- 권한 부여는 **항상 인증 이후**에 발생합니다.
- 사용자 **권한이나 역할이 필요**합니다.
- 권한 부여에 실패할 경우, 일반적으로 **403 오류 응답**을 받게 됩니다.
- 애플리케이션에 로그인한 후, 내 역할이나 권한이 어떤 작업을 수행할 수 있는지를 결정합니다.


## 실제 사례 : 여행 예시
  - **주 출입구에서 인증**이 이루어지고, **공항 내부의 게이트에서는 인가**가 이루어집니다. 예를 들어, 사용자가 런던행 비행기가 있는 4번 게이트로 가면 인가가 성공합니다. 이는 사용자가 런던으로 여행할 충분한 권한을 가지고 있기 때문입니다.


# 87. 스프링 시큐리티 내에서 권한이 저장되는 방식
## 인가 흐름 과정 ✔
인가를 구현하려고 하므로 권한(authority) 또는 역할(role)이 Spring Security 프레임워크 내에서 어떻게 저장되는지 이해해 보겠습니다.

▶ UserDetails 흐름
`UserDetailsService` -> `UserDetails` -> `GrantedAuthority`

▶ Authentication 흐름
`AuthenticationProvider` -> `Authentication` ->  `GrantedAuthority`


## GrantedAuthority 인터페이스
- **인증 객체에 모든 권한 또는 역할의 세부 정보를 String 데이터 타입을 사용하여 저장**할 것입니다. 
- Security팀은 `SimpleGrantedAuthority`로 구현 심플을 제공했습니다.
```java
public interface GrantedAuthority extends Serializable {
	String getAuthority();
}
```


## 인가 구현 코드 추가 ✔
▶ UserDetails 인터페이스
저장 시스템에서 **권한을 `UserDetails` 구현 클래스의 객체를 사용하여 저장**합니다.
```java
@Service  
@RequiredArgsConstructor  
public class EazyBankUserDetailsService implements UserDetailsService {  
  
    private final CustomerRepository customerRepository;  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        Customer customer = customerRepository.findByEmail(username).orElseThrow(() -> new  
                UsernameNotFoundException("User details not found for the user: " + username));  
        List<GrantedAuthority> authorities = List.of(new SimpleGrantedAuthority(customer.getRole()));  
        
        return new User(customer.getEmail(), customer.getPwd(), authorities);  
    }  
}
```
- 한 사람이 여러 권한 또는 역할을 가질 수 있기 때문에 `List<GrantedAuthority>` 객체 형태로 권한 세부 정보를 받아들입니다


▶ Authentication 인터페이스
인증이 성공하면 로그인한 **사용자 정보를 `Authentication` 구현 클래스 객체의 형태로 저장**할 것입니다.
```java
public class EazyBankProdUsernamePwdAuthenticationProvider implements AuthenticationProvider {  
  
    private final UserDetailsService userDetailsService;  
    private final PasswordEncoder passwordEncoder;  
  
    @Override  
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {  
        String username = authentication.getName();  
        String pwd = authentication.getCredentials().toString();  
        UserDetails userDetails = userDetailsService.loadUserByUsername(username);  
        if (passwordEncoder.matches(pwd, userDetails.getPassword())) {  
            // 비즈니스 로직 구현 가능
            return new UsernamePasswordAuthenticationToken(username,pwd,userDetails.getAuthorities());  
        }else {  
            throw new BadCredentialsException("Invalid password!");  
        }  
    }
	
	...
}
```


## User에서의 권한 살펴보기
```java
public User(String username, .. , 
       Collection<? extends GrantedAuthority> authorities) {  
    this.username = username;  
	... 
    this.authorities = Collections.unmodifiableSet(sortAuthorities(authorities));  
}
```
- Spring Security팀은 **권한이 한 번 생성되면 수정할 수 없는 고유한 요소들로 구성된 세트**를 만들었습니다.
- 동일한 권한 컬렉션 객체는 `getAuthorities()` 메소드를 호출할 때마다 반환됩니다.


## Authentication에서의 권한 살펴보기
```java
public AbstractAuthenticationToken(Collection<? extends GrantedAuthority> authorities) {  
    if (authorities == null) {  
       this.authorities = AuthorityUtils.NO_AUTHORITIES;  
       return;  
    }  
    for (GrantedAuthority a : authorities) {  
       Assert.notNull(a, "Authorities collection cannot contain any null elements");  
    }  
    this.authorities = Collections.unmodifiableList(new ArrayList<>(authorities));  
}
```
- Spring Security팀은 **권한이 한 번 생성되면 수정할 수 없는 고유한 요소들로 구성된 세트**를 만들었습니다.


# 88. 다양한 역할 또는 권한을 저장하기 위한 새로운 authorities 테이블 만들기
Spring Security는 **단일 사용자에 대해 여러 역할과 권한을 저장할 수 있는 기능 제공**합니다. DB 스키마 변경을 통해 사용자에 대한 여러 역할 또는 권한 저장 해봅시다.

## authorities 테이블 생성 ✔
```sql
CREATE TABLE `authorities` (  
	`id` int NOT NULL AUTO_INCREMENT,  
	`customer_id` int NOT NULL,  
	`name` varchar(50) NOT NULL,  
	PRIMARY KEY (`id`),  
	KEY `customer_id` (`customer_id`),  
	CONSTRAINT `authorities_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`customer_id`)  
);
```
- 이 테이블은 customer 테이블과 외래 키 관계를 가집니다.


## 권한 및 역할 정보 삽입
```sql
INSERT INTO `authorities` (`customer_id`, `name`)  VALUES (1, 'ROLE_USER');  
INSERT INTO `authorities` (`customer_id`, `name`)  VALUES (1, 'ROLE_ADMIN');
```


## DB에 권한이 구성되어 있지 않은 사용자가 있다면?
Spring Security는 해당 사용자에게 **403 에러를 반환**할 것입니다


# 89. 새 데이터베이스 테이블에서 권한을 로드하기 위한 백엔드 변경 사항 적용하기
Customer 테이블에서 **여러 권한을 갖기 위한 설정**을 배워봅시다.

## Authority 클래스 생성 ✔
**`GrantedAuthority`를 인터페이스를 구현하지 않고** 비즈니스 로직에서 `SimpleGrantedAuthority`을 생성해서 권한 부여할 것입니다.
```java
@Entity  
@Getter @Setter  
@Table(name="authorities")  
public class Authority {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private long id;  
  
    private String name;  
  
    @ManyToOne  
    @JoinColumn(name="customer_id")  
    private Customer customer;  
  
}
````
- `@ManyToOne`: 여러 권한이 하나의 고객에 연결되도록 설정합니다.
- `@JoinColumn(name = "customer_id")`: authorities 테이블의 customer_id 열을 외래 키로 설정합니다.


## Customer 엔티티 클래스 변경 ✔
Customer 엔티티에 **권한 정보를 추가**합니다. 
```java
@Entity  
@Getter @Setter  
public class Customer {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @Column(name = "customer_id")  
    private long id;  
  
    ...
  
    @OneToMany(mappedBy = "customer", fetch = FetchType.EAGER)  
    @JsonIgnore  
    private Set<Authority> authorities;  
  
}
```
- `Set<Authority> authorities`: 고객이 가질 수 있는 권한 리스트를 저장합니다.
- `@OneToMany(mappedBy = "customer", fetch = FetchType.EAGER)`: 고객이 여러 권한을 가질 수 있음을 명시합니다. `mappedBy`는 `Authority` 클래스에서 참조하는 필드 이름을 지정합니다.
- `@JsonIgnore` : UI가 권한 정보를 필요로 하지 않음을 명시합니다. (순환참조 해결)


## DetailService 수정 ✔
`findByEmail()` 메소드에서 **`Customer` 객체를 로드할 때 권한 정보도 함께 로드**됩니다. 기존의 역할 열을 사용하지 않고, `getAuthorities()` 메소드를 통해 권한 정보를 가져옵니다.
```java
@Service  
@RequiredArgsConstructor  
public class EazyBankUserDetailsService implements UserDetailsService {  
  
    private final CustomerRepository customerRepository;  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        Customer customer = customerRepository.findByEmail(username).orElseThrow(() -> new  
                UsernameNotFoundException("User details not found for the user: " + username));  

		// 권한 리스트 만들기
        List<GrantedAuthority> authorities = customer.getAuthorities().stream().map(authority -> new  
                        SimpleGrantedAuthority(authority.getName())).collect(Collectors.toList());  

        return new User(customer.getEmail(), customer.getPwd(), authorities);  
    }  
}
```
- `Authority` 객체를 **`SimpleGrantedAuthority`로 변환하여 Spring Security의 권한 시스템과 통합**합니다.


## 테스트 및 검증
- 브라우저에서 로그인 : 로그인 시 대시보드 페이지에 접근 여부 확인
- Postman을 사용한 테스트 : API 요청 시 권한 로드가 제대로 작동하는지 확인


# 90. 스프링 시큐리티를 사용하여 웹 애플리케이션 내에서 권한 구성하기
Spring Security 프레임워크에서 권한을 사용하여 권한 부여를 구현하는 방법을 이해했습니다. **각 API에 대한 권한 부여 규칙을 설정**하고, 다양한 테스트 시나리오를 통해 확인했습니다.


## 권한에 따른 API 보호 설정 ✔
- 기존에는 인증된 사용자만 API에 접근할 수 있도록 `authenticated()` 메소드를 호출했습니다.
- 하지만, 이제는 권한에 따라서 각 API에 대해 특정 권한을 요구하도록 설정합니다.
```java
http.authorizeHttpRequests((requests) -> requests  
	.requestMatchers("/myAccount").hasAuthority("VIEWACCOUNT")  
	.requestMatchers("/myBalance").hasAnyAuthority("VIEWBALANCE", "VIEWACCOUNT")  
```
- `hasAuthority()` 메소드 : 사용자가 **권한을 가진 경우** API 접근 허용.
- `hasAnyAuthority()` 메소드 : 사용자가 **여러 권한 중 하나라도 가진 경우** API 접근 허용.
- `access()` 메소드 : Spring 표현 언어(SPEL)를 활용한 **복잡한 요구 사항을 설정**할 수 있습니다.
![[Spring_Security_87 1.png]]


## 테스트
- 권한이 없는 경우 403 Forbidden 에러 발생합니다. 원래는 오류페이지가 발생하지만, 핸들러로 오류 처리했습니다.


## CORS 및 프로파일 설정
- HTTPS를 사용하도록 설정했으나, CORS 정책에서 HTTP를 언급한 경우 문제가 발생할 수 있습니다. **로컬 테스트 시 HTTP를 사용**하고, **프로덕션에서는 HTTPS를 강제하도록 설정**합니다.


# 91. 스프링 시큐리티에서 권한 vs 역할
## 권한 VS 역할 ✔
▶ 권한 (Authority)
- **개별 권한 또는 행동**을 나타냅니다. 특정 행동이나 호출에 대한 접근을 제어합니다.
 - 예: `VIEWACCOUNT`, `VIEWCARDS` 등.

▶ 역할 (Role)
- **권한의 그룹**을 나타냅니다. **여러 권한을 하나의 역할로 묶어 관리**합니다.
- `ROLE_USER`: `VIEWACCOUNT`, `VIEWCARDS`
- `ROLE_ADMIN`: `DELETEACCOUNT`, `UPDATEACCOUNT`, `VIEWCARDS`


## 권한과 역할이 생긴 이유는? ✔
- **구성의 간소화** : 수천 개의 개별 권한을 관리하는 대신, 역할을 통해 권한을 그룹화하여 관리의 복잡성을 줄입니다.
- **접근 제어 방식 정밀화** : 작은 단위 방식 권한을 사용하여 세밀하게 접근 제어 가능하게 합니다.


## Spring Security 내에서의 규칙 ✔
- 권한과 역할은 모두 **`GrantedAuthority`라는 인터페이스를 구현**합니다.
- 권한은 직접적으로 사용하며, **역할**을 정의할 때는 **반드시 `ROLE_` 접두사를 사용**해야 합니다.
- 예: `VIEWACCOUNT`, `ROLE_ADMIN`, `ROLE_USER`


# 92. 스프링 시큐리티를 사용하여 웹 애플리케이션 내에서 역할 인가 구성하기

## DB에서 권한 -> 역할 바꾸기
1) 기존 권한을 삭제하고 새로운 역할을 삽입합니다.
```sql
DELETE FROM authorities; -- 권한 삭제
INSERT INTO roles (customer_id, role_name) VALUES (1, 'ROLE_USER'); -- 역할 추가
INSERT INTO roles (customer_id, role_name) VALUES (1, 'ROLE_ADMIN'); -- 역할 추가
````

2) 비즈니스 로직 구축하기
[[Spring Security] 역할/권한 구현하기 (tistory.com)](https://backend-jaamong.tistory.com/168)
모든 신규 사용자에게 미리 정의된 권한 또는 기본 권한이나 기본 역할을 부여하는 비즈니스 로직을 구축할 것입니다


## ProjectSecurityConfig 클래스 수정 ✔
- 기존의 권한 관련 메소드에서 역할 관련 메소드로 변경합니다.
```java
http.authorizeHttpRequests((requests) -> requests  
	.requestMatchers("/myAccount").hasRole("USER")  
	.requestMatchers("/myBalance").hasAnyRole("USER", "ADMIN")  
	.requestMatchers("/myLoans").hasRole("USER")  
	.requestMatchers("/myCards").hasRole("USER")  
	.requestMatchers("/user").authenticated()
```
- Spring Security에서는 역할을 구성할 때 `ROLE_` 접두사를 명시할 필요가 없습니다. 메소드 호출 시 **자동으로 `ROLE_`이 추가**됩니다. (`hasRole` 주석 참고)
- 예시) 잘못된 사용: `hasRole("ROLE_USER")`, 올바른 사용: `hasRole("USER")`


## 테스트 및 검증
- `john@example.com`으로 로그인: 403 에러 발생 (역할 없음).
- `happy@example.com`으로 로그인: 정상 응답 (USER 역할 보유).


# 93. 인가 이벤트 수신하기
## 권한 부여 실패 시 403 에러 처리
- Spring Security는 **권한 부여 실패 시** 403 에러 응답을 반환합니다.
- **클라이언트 측**에서는 이 에러에 기반하여 오류 메시지를 표시하거나 다른 페이지로 리디렉션하는 로직을 구현할 수 있습니다.
- 백엔드에서 비즈니스 로직을 이용해서 다양하게 처리할 수 있습니다. (이메일 알림 전송, 데이터베이스에 감사 항목 추가, 오류 로그 작성, 이벤트를 게시)


## AuthorizationDeniedEvent 수신 ✔
- `AuthorizationDeniedEvent`을 이용해서 **권한오류 발생시 이벤트를 게시**하겠습니다. 
```java
@Component  
@Slf4j  
public class AuthorizationEvents {  
  
    @EventListener  
    public void onFailure(AuthorizationDeniedEvent deniedEvent) {  
        log.error("Authorization failed for the user : {} due to : {}", deniedEvent.getAuthentication().get().getName(),  
                deniedEvent.getAuthorizationDecision().toString());  
    }  
  
}
````


## 테스트
`john@example.com` 사용자로 API 호출 시 403 에러 발생. 중단점에서 디버깅하여 권한 부여 실패 로그 확인.


## AuthorizationGrantedEvent
- 권한 부여 승인 이벤트 처리는 기본적으로 게시하지 않음.이유는 애플리케이션 내의 **수천 개 작업으로 인해 노이즈 발생**.