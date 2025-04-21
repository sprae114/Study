# 스프링 시큐리티
## FOR USE
#### 1) 시큐리티 사용하기전에 의존성을 넣기
```gradle
dependencies {  
	...
    implementation 'org.springframework.boot:spring-boot-starter-security'  
	...
}
```

#### 2) SecurityConfig에서 보안 설정
- `securityFilterChain` : **HTTP 요청**에 대한 보안 필터 체인을 정의하는 역할을 함.
정적 리소스에 대한 요청은 모두 허용
`GET` 메서드를 사용하는 "/", "/articles", "/articles/search-hashtag" 경로에 대한 요청은 모두 허용함
나머지 요청은 인증된 사용자만 접근할 수 있도록 합니다. (`authenticated()`)
로그아웃 성공 시 리다이렉트되는 URL을 "/"로 설정

- `UserDetailsService` :사용자 이름을 기반으로 사용자 정보를 로드하는 데 사용함.
왜 로드 하는거야?  **사용자 계정 저장소(DB)에서 사용자 정보를 조회**하고, 조회된 사용자 정보를 `UserDetails` **구현체로 변환**하는 과정을 수행합니다.


#### 3) Principal 객체 만들기
사용자 **인증 및 인가에 필요한 사용자 정보**를 제공합니다.


#### 4) JpaConfig
**현재 인증된 사용자의 이름을 반환하는 구현체를 제공**합니다.

- 테스트에 사용할때 문제점이 발생함.
테스트 환경에서는 실제 애플리케이션의 보안 컨텍스트와 **인증 객체가 사용되지 않을 수 있습니다.**
테스트의 독립성과 **일관성을 유지하기 할 수 없음**. 그래서 Test용 JpaConfig를 따로 만듦


#### 5) Repository에 적용
id만 사용 -> id  + name 사용


#### 6) Service에 적용
**이전에는 권한설정이 없어서 누구나 기능을 사용할 수** 있었다. 예를 들어, 게시글을 삭제한다고 했을때, 게시글 번호만 찾아서 삭제하는 기능이었다. 여기서 인증된 사용자 이름을 넣어서 **이름이 같을 때만 삭제할 수 있게 수정**함.

찾을 수 없는 경우, `Exception`이 발생하며, 이 경우 로그를 남기고 작업을 중지합니다.


#### 7) Controller에 적용
**`@AuthenticationPrincipal`** 을 활용해서 현재 인증된 사용자의 정보를 `Principal` 타입의 객체로 주입합니다. 이를 통해 현재 로그인한 사용자의 정보를 가져올 수 있습니다.

이제 서비스 로직에 인증된 사용자의 정보를 함께 전달하여 새로운 기능을 동작합니다.


## FOR TEST
#### 1) JPA 연결 테스트
위에서 말했다시피 테스트용 `TestJpaConfig` 생성.

#### 1) HTTP 보안설정
- HTTP 경로 대한 요청이 허용 O 
=> 인증 상관없이 기존의 테스트 코드로 하면됌

- HTTP 경로 대한 요청이 허용 X
▶`@AutoConfigureMockMvc`
**Spring MVC 테스트 환경을 자동으로 설정**합니다. 이 어노테이션을 사용하면 **MockMvc 인스턴스를 주입받아 컨트롤러와 상호 작용하면서 인증 및 인가를 테스트**할 수 있습니다.
```java
@SpringBootTest
@AutoConfigureMockMvc
public class SecurityTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testSecuredEndpoint() throws Exception {
        mockMvc.perform(get("/secured"))
               .andExpect(status().isForbidden());
    }
}
```

▶`@WithMockUser`
**가상의 인증된 사용자를 만들어** 테스트에 사용합니다. 이 어노테이션을 사용하면 **특정 사용자 이름, 권한 등을 지정**할 수 있습니다.
```java
@Test
@WithMockUser(username = "testuser", roles = {"USER"})
public void testWithMockUser() {
    // 이 테스트에서는 testuser라는 가상의 인증된 사용자로 실행됩니다.
}
```


▶`@WithUserDetails`
**실제 사용자 정보를 로드하여 테스트에 사용**합니다. 이 어노테이션은 사용자 정보를 **`UserDetailsService`를 통해 로드**하며, 사용자 이름을 지정할 수 있습니다.
```java
@Test
@WithUserDetails("testuser")
public void testWithUserDetails() {
    // 이 테스트에서는 testuser의 사용자 정보를 로드하여 실행됩니다.
}
```


▶`@WithSecurityContext`
사용자 정의 보안 컨텍스트를 설정할 수 있는 어노테이션입니다. 이 어노테이션을 사용하면 사용자 정의 `SecurityContext`를 만들어 테스트에 사용할 수 있습니다.
```java
@Test
@WithSecurityContext(factory = CustomSecurityContextFactory.class)
public void testWithCustomSecurityContext() {
    // 이 테스트에서는 CustomSecurityContextFactory를 사용하여 보안 컨텍스트가 설정됩니다.
}
```


#### 2) 허용된 사용자에 데이터 접근


#### 3) data.sql이 작동되는 지 확인


# 카카오 인증
[[Spring Boot] 카카오 로그인 API 구현 (1) - Access token 발급받기 — 수바리의 코딩일기 (tistory.com)](https://suyeoniii.tistory.com/79)
[(1) WEB2 - OAuth 2.0 : 1.수업소개 - YouTube](https://www.youtube.com/watch?v=hm2r6LtUbk8&list=PLXB5p_g4hZpMgHGZ4iMefHl_6D51sEhaT&index=1)
## FOR USE
#### 1) kakao Developers에서 카카오 API 사용 준비하기

#### 2) 의존성 및 프로퍼티 설정하기

#### 3) Principal를 OAuth 인증 정보를 다룰 수 있도록 기능 추가 및 회원 DTO 수정
기존에는 UserDatils 객체를 구현했지만, OAuth 객체를 추가해서 구현해야함. 

#### 4) 회원 정보 서비스 로직의 구현

#### 5) SecurityConfig에 OAuth 로그인 기능 추가
기존 DB 인증에다가 OAuth 인증까지 받아들일 수 있도록 설정 추가


## FOR TEST
#### 1) Kakao OAuth 2.0 인증 응답 데이터 테스트
카카오 인증 API 의 결과값 사용

#### 2) JPA 연결 테스트
테스트용 `TestJpaConfig` 생성.

#### 3) 회원 정보 서비스 테스트

#### 4) TestSecurityConfig
Repository에서 Service로 바뀐 비즈니스 로직으로 변경함.

