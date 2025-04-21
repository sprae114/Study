# 요약
[퀴즈](https://www.udemy.com/course/spring-security-6-jwt-oauth2-korean/learn/quiz/6534461#overview)
- `InMemoryUserDetailsManager`사용시 유저를 저장할 수 있음(p.23)
- 비밀번호를 강제 강화할 수 있음(p.24)
- `UserDetailsService`를 이해할 수 있음(p.25)
- `UserDetails` 과 `Authentication`의 차이를 알 수 있음(p.26)

# 22. InMemoryUserDetailsManager를 사용한 사용자 구성하기
실제 프로젝트에서는 **여러 사용자를 생성할 수 있는 방법**이 필요합니다.

## 사용자 인증 개선 ✔
- **단일 사용자 계정**: 단일 사용자가 사용할 수 있는 애플리케이션을 만드는 것은 실제로 불가능합니다.
- **다중 사용자 지원**: 애플리케이션 메모리 내에서 여러 사용자 계정을 생성하는 방법을 설명합니다.


## 단일 사용자 삭제
- `application.properties`에서 사용자 자격 증명 관련 속성을 삭제합니다.

## 시큐리티 흐름 ✔
![[Pasted image 20230827204157.png]]
![[Spring_Security_18 2.png]]
- `DaoAuthenticationProvider`는 Spring Security에서 제공하는 인증 제공자 중 하나로, **데이터베이스에 저장된 사용자 정보를 사용하여 인증을 처리**하는 데 사용됩니다.


## UserDetailsService ✔
**유저 특정 데이터를 저장 시스템에서 불러오는 데 사용**됩니다. 비밀번호가 노출되는 것을 방지하기 위해 **유저 이름만으로 세부 정보를 로드하는 방식**이 권장됩니다.
```java
public interface UserDetailsService {  
	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;  
}
```


## InMemoryUserDetailsManager ✔
- Spring Security 프레임워크의 도움으로 애플리케이션 **메모리 내에 여러 사용자를 생성**하는 방법입니다. 
- `@Bean` 어노테이션을 사용하여 `UserDetailsService` 타입의 Bean을 생성합니다.
- `User` 클래스를 사용하여 사용자 정보를 설정합니다.
```java
@Bean
public UserDetailsService userDetailsService() {
    InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
    UserDetails user = User.withUsername("user")
                            .password("{noop}12345") // noop 접두사 사용
                            .authorities("read")
                            .build();
    manager.createUser(user);
    
    UserDetails admin = User.withUsername("admin")
                             .password("{noop}54321") // noop 접두사 사용
                             .authorities("admin")
                             .build();
    manager.createUser(admin);
    
    return manager;
}
````
- 비밀번호의 인코더를 설정하지 않으면 오류가 나타납니다. 그래서 위 코드에서 **`noop` 접두사를 사용하여 비밀번호를 평문으로 처리**합니다. 이는 권장되지 않습니다. 


## UserDetails ✔
- Spring Security 내 인터페이스로 **사용자**를 나타냅니다
- 이 인터페이스는 **인증된 사용자에 대한 정보를 제공**하며, Spring Security의 인증 및 권한 부여 프로세스에서 중요한 역할을 합니다.
```java
public interface UserDetails {
    String getUsername(); // 사용자 이름 반환
    String getPassword(); // 비밀번호 반환
    Collection<? extends GrantedAuthority> getAuthorities(); // 사용자의 권한 반환
    boolean isAccountNonExpired(); // 계정 만료 여부
    boolean isAccountNonLocked(); // 계정 잠금 여부
    boolean isCredentialsNonExpired(); // 자격 증명 만료 여부
    boolean isEnabled(); // 계정 활성화 여부
}
```



# 23. PasswordEncoderFactories를 사용한 PasswordEncoder 구성하기
유저를 생성할때, 보완에 취약하지 않게 **비밀번호를 안전하게 관리하는 방법**을 배워보자.

## 문제점
1. **자격 증명 하드코딩**: 소스 코드에 직접 비밀번호를 저장하면, 누구나 소스 코드에 접근할 수 있는 경우 비밀번호를 쉽게 알 수 있습니다.
2. **평문 비밀번호 저장**: 현재 비밀번호가 애플리케이션 메모리 내에 평문으로 저장되어 있어, 해커가 힙 덤프에 접근할 경우 쉽게 알아낼 수 있습니다.


## 해결책
- **PasswordEncoder 사용**: Spring Security 프레임워크의 `PasswordEncoder`를 사용하여 비밀번호를 암호화 또는 해싱 형식으로 저장합니다.
![[Spring_Security_26 1.png]]

## PasswordEncoder 생성 ✔
- 새로운 메소드를 생성하여 `PasswordEncoder` 타입의 Bean을 반환합니다.
- `PasswordEncoderFactories.createDelegatingPasswordEncoder()`를 사용하여 기본 PasswordEncoder를 생성합니다. 기본적으로 `BCryptPasswordEncoder`가 사용됩니다.
```java
@Bean
public PasswordEncoder passwordEncoder() {
    return PasswordEncoderFactories.createDelegatingPasswordEncoder();
}
````


## 다른 PasswordEncoder를 사용하고 싶다면?
- `createDelegatingPasswordEncoder` 메소드 안에는 다양한 인코딩 방법이 생성되어 있음.
```java
public static PasswordEncoder createDelegatingPasswordEncoder() {  
	String encodingId = "bcrypt";  
	Map<String, PasswordEncoder> encoders = new HashMap<>();  
	encoders.put(encodingId, new BCryptPasswordEncoder());  
	encoders.put("ldap", new LdapShaPasswordEncoder());  
	encoders.put("noop", NoOpPasswordEncoder.getInstance());  
	
	...
	
	return new DelegatingPasswordEncoder(encodingId, encoders);  
}
```

- 접두사를 사용하여 다른 PasswordEncoder를 사용하자. 예를 들어, `"{bcrypt}"`로 시작하는 비밀번호는 BCrypt 알고리즘으로 처리하고, `"{noop}"`로 시작하는 비밀번호는 평문으로 처리합니다.
```java
@Bean
public UserDetailsService userDetailsService() {
    InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
    UserDetails user = User.withUsername("user")
                            .password("{noop}12345") // 평문 비밀번호
                            .authorities("read")
                            .build();
    manager.createUser(user);
    
    UserDetails admin = User.withUsername("admin")
                             .password("{bcrypt}$2a$10$...") // 해시된 비밀번호
                             .authorities("admin")
                             .build();
    manager.createUser(admin);
    
    return manager;
}
```


# 24. CompromisedPasswordChecker 데모
**간단한 비밀번호 사용을 막는 방법**을 알아보겠습니다.

## 비밀번호 검증 ✔
[나는 Pwned 되었는가 : API v3 (haveibeenpwned.com)](https://haveibeenpwned.com/API/v3#PwnedPasswords)
![[Spring_Security_27.png]]
- `HaveIBeenPwnedRestApiPasswordChecker`를 사용하여 비밀번호가 유출되었는지 확인합니다.
- **간단한 비밀번호를 사용하지 않도록 강제하기 위해 비밀번호를 강력한 비밀번호로 변경**합니다.


# 25. UserDetailsService 및 UserDetailsManager 인터페이스에 대한 심층 탐구

## 내부 흐름 개요 ✔
- 인증 제공자는 인증을 수행하기 위해 `PasswordEncoder`와 `UserDetailsManager` 또는 `UserDetailsService`를 활용합니다.
![[Spring_Security_13.png]]


## UserDetails ✔
- Spring Security 내 인터페이스로 **사용자**를 나타냅니다
- 이 인터페이스는 **인증된 사용자에 대한 정보를 제공**하며, Spring Security의 인증 및 권한 부여 프로세스에서 중요한 역할을 합니다.
```java
public interface UserDetails {
    String getUsername(); // 사용자 이름 반환
    String getPassword(); // 비밀번호 반환
    Collection<? extends GrantedAuthority> getAuthorities(); // 사용자의 권한 반환
    boolean isAccountNonExpired(); // 계정 만료 여부
    boolean isAccountNonLocked(); // 계정 잠금 여부
    boolean isCredentialsNonExpired(); // 자격 증명 만료 여부
    boolean isEnabled(); // 계정 활성화 여부
}
```



## UserDetailsService 인터페이스란? ✔
- **유저 특정 데이터를 저장 시스템에서 불러오는 데 사용**됩니다. 비밀번호가 노출되는 것을 방지하기 위해 **유저 이름만으로 세부 정보를 로드하는 방식**이 권장됩니다.
```java
public interface UserDetailsService {  
	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;  
}
```


## UserDetailsManager 인터페이스란?
- **UserDetailsManager**는 UserDetailsService를 확장한 인터페이스로, 유저를 관리하는 다양한 메소드를 제공합니다.
- **메소드 목록** (CRUD)
```
changePassword : 엔드 유저가 자신의 비밀번호를 변경할 수 있게 해줍니다.
createUser : 새로운 유저를 등록할 때 사용합니다.
deleteUser : 유저를 삭제합니다.
updateUser : 유저의 프로필 세부 정보를 업데이트합니다.
userExists : 주어진 유저 이름으로 시스템 내에 유저가 존재하는지 확인합니다.
```


## UserDetailsManager 구현 클래스 주요 구현 클래스 ✔
Security에서 기본으로 제공하는 UserDetailsManager  세 가지 구현클래스가 가장 많이 사용되며, 이들에 대해 배우면 전체적인 그림을 이해하는 데 도움이 됩니다.
1. **InMemoryUserDetailsManager**
2. **JdbcUserDetailsManager**
3. **LdapUserDetailsManager**

![[Pasted image 20240926164923.png]]


## InMemoryUserDetailsManager 코드 탐구
1) createUser 메소드
- 사용자가 이 클래스의 생성자를 호출할 때마다 호출됩니다.
- 유저 정보를 `users`라는 이름의 맵에 저장합니다.

2) deleteUser 메소드
- 지정된 키를 통해 HashMap에서 특정 유저를 삭제합니다.

3) updateUser 메소드
- 유저의 정보를 업데이트합니다.

4) userExists 메소드
- 특정 유저 이름을 가진 유저가 존재하는지 확인하고, 존재하면 `true`, 아니면 `false`를 반환합니다.

5) changePassword 메소드
- 유저의 비밀번호를 변경하는 로직이 포함되어 있습니다.

6) loadUserByUsername 메소드
- 저장 시스템에서 유저 정보를 로드하는 메소드입니다.


## UserDetailsManager 구현체 주입 방법 ✔
**ProjectSecurityConfig**에서 `InMemoryUserDetailsManager` 타입의 Bean을 생성하였습니다. 이는 Spring 프레임워크에 `InMemoryUserDetailsManager`를 사용하라는 지시입니다.

```java
@Bean  
public UserDetailsService userDetailsService() {  
	// 사용자 주입
    return new InMemoryUserDetailsManager();  
}
```



## JdbcUserDetailsManager 코드 탐구 ✔
- 데이터베이스에서 **유저 정보를 로드할 때 사용**됩니다. `loadUserByUsername` 메소드는 SQL 쿼리를 생성하여 유저 정보를 검색합니다.
- 기본적으로 **Spring Security 팀이 데이터베이스 구조를 설계**하였습니다. `users` 테이블은 유저 이름, 비밀번호, 활성화 상태를 저장합니다.
- `createUserSql`: 데이터베이스에 유저를 생성하는 SQL `INSERT` 문. 삭제 및 업데이트 메소드도 유사하게 작동합니다.


## 자체 인증 로직 작성 ✔
자체 인증 로직이 있거나 모든 것을 직접 작성하려는 경우 **`AuthenticationProvider`를 구현해야함. 지금은 `DaoAuthenticationProvider`라는 기본값 사용**

![[Pasted image 20240926170453.png]]


# 26. UserDetails 및 Authentication 인터페이스에 대한 심층 탐구

## UserDetails 인터페이스의 메소드 설명 ✔
1. **getAuthorities()**
   - 엔드 유저의 권한 또는 역할 목록을 반환합니다.
   
2. **getPassword()**
   - 엔드 유저의 비밀번호를 반환합니다.
   
3. **getUsername()**
   - 엔드 유저의 유저 이름을 반환합니다.

4. **계정 상태 확인 메소드**
   - `isAccountNonExpired`: 계정이 만료되지 않았는지 확인
   - `isAccountNonLocked`: 계정이 잠기지 않았는지 확인
   - `isCredentialsNonExpired`: 자격 증명이 만료되지 않았는지 확인
   - `isEnabled`: 계정이 활성화되어 있는지 확인


## User 클래스란? ✔
- Spring Security 팀은 **`UserDetails` 인터페이스의 샘플 구현**을 제공합니다.
- User 객체 생성 시 비밀번호, 유저 이름, 권한을 전달하며, 계정 상태는 기본값으로 설정됩니다.
- User 클래스 내에는 **읽기 전용**의 getter 메소드만 존재하며, setter 메소드는 없습니다.


## Authentication 란?
- `Principal` 인터페이스를 확장하며, **유저의 인증 상태를 확인**하는 메소드가 포함되어 있습니다.


## Authentication과 UserDetails의 관계 ✔
- **UserDetails**: 사용자 **세부 정보**를 담고 있는 인터페이스로, 구현 클래스인 `User`를 통해 표현됩니다. 비밀번호, 사용자 이름, 권한, 계정 상태 등을 확인할 수 있는 메소드가 포함되어 있습니다.
  
- **Authentication**: **인증 상태**를 나타내는 인터페이스로, `Principal` 인터페이스를 확장합니다. `getName()`, `getCredentials()`, `isAuthenticated()`와 같은 메소드가 포함되어 있습니다.
![[Pasted image 20240926175641.png]]


## Provider와 UserDetailsManager의 상호 작용
- `AuthenticationProviders`는 인증이 성공하면 `UserDetails`를 **인증 객체로 변환**합니다.
![[Spring_Security_18 1.png]]