# 요약
[퀴즈](https://www.udemy.com/course/spring-security-6-jwt-oauth2-korean/learn/quiz/6534495#overview)

- 인코딩, 암호화, 해싱의 차이를 알 수 있음(p.36~41)
- 스프링에서 `PasswordEncoder`가 동작하는 방식을 알 수 있음(p.42~43)
- `PasswordEncoder`를 회원가입과 로그인에서 사용할 수 있음(p.44)

# 35. PasswordEncoder 없이 비밀번호가 어떻게 검증되는지
Spring Security 프레임워크에서 **비밀번호를 안전하게 관리**하기 위해 `PasswordEncoder`를 사용하는 방법을 설명하겠습니다.

## 기본 PasswordEncoder의 문제점 ✔
현재 우리는 비밀번호를 **일반 텍스트 형식으로 처리**하고 있는 기본 PasswordEncoder를 사용하고 있습니다. 이 접근법은 여러 가지 문제를 야기할 수 있습니다:

- **보안 위협**: 데이터베이스 **관리자(DBA)나 해커가 비밀번호를 쉽게 조회**할 수 있습니다.
- **무결성 및 기밀성 문제**: 비밀번호가 노출되면 **사용자 계정이 위험**에 처할 수 있습니다.


## REST API와 비밀번호 관리 ✔
- 이전 섹션에서 `register`라는 이름의 REST API를 생성하여 최종 사용자 정보를 데이터베이스에 저장하는 방법을 배웠습니다.
- 사용자가 평문 비밀번호를 입력하면, 이를 **Security팀에서 추천하는 `BcryptPasswordEncoder`를 사용하여 해시한 후 데이터베이스에 저장**합니다.


## PasswordEncoder 설정 ✔
- `PasswordEncoder`는 `ProjectSecurityConfig`에서 정의됩니다. Spring Security는 기본적으로 `BcryptPasswordEncoder`를 사용하도록 설정되어 있습니다.
```java
@Bean
public PasswordEncoder passwordEncoder() {
    return PasswordEncoderFactories.createDelegatingPasswordEncoder();
}
```

- `createDelegatingPasswordEncoder` 안에서 encoder 골라서 사용하면 됨.
```java
public static PasswordEncoder createDelegatingPasswordEncoder() {  
   String encodingId = "bcrypt";  
   Map<String, PasswordEncoder> encoders = new HashMap<>();  
   encoders.put(encodingId, new BCryptPasswordEncoder());  
   encoders.put("noop", NoOpPasswordEncoder.getInstance());  
   ...
    
   return new DelegatingPasswordEncoder(encodingId, encoders);  
}
```


## 인증 과정에서 PasswordEncoder의 사용
- 사용자가 로그인할 때 입력한 비밀번호는 `PasswordEncoder`를 사용하여 해시된 비밀번호와 비교됩니다.
- Spring Security는 저장된 비밀번호의 접두사를 확인하여 적절한 `PasswordEncoder`를 사용합니다.


# 36. 인코딩과 디코딩이란 무엇이며, 왜 비밀번호 관리에 적합하지 않은지

이번 강의에서는 비밀번호를 **안전하게 관리하기 위한 다양한 방법**에 대해 설명하겠습니다. 특히 인코딩, 암호화, 해싱의 차이와 각각의 사용 사례를 다룰 것입니다.

## 인코딩 vs 암호화 vs 해싱 그림비교 ✔
![[Spring_Security_36.png]]

## 인코딩 ✔
- 정의: 데이터를 **한 형태에서 다른 형태로 변환하는 과정.**
- 특징: 인코딩은 비밀이 포함되지 않으며, **누구나 원본 데이터를 쉽게 복원**할 수 있습니다.
- 예시: ASCII, BASE64, UNICODE 등.
- 비밀번호 관리에 적합성: 인코딩은 비밀번호 관리에 권장되지 않습니다. 해커가 원본 데이터를 쉽게 복원할 수 있기 때문입니다


# 37. 암호화와 복호화란 무엇이며, 왜 비밀번호 관리에 적합하지 않은지
비밀번호 관리에 대한 암호화의 적합성에 대해 논의하고, 암호화의 개념과 그 한계를 설명하겠습니다.

## 암호화란? ✔
- 암호화는 **데이터를 기밀하게 보호하기 위해 변환**하는 과정입니다.
- 암호화는 비밀 키를 사용하여 데이터를 변환하며, **암호화된 데이터는 동일한 알고리즘과 키를 사용하여 복호화**할 수 있습니다.


## 암호화의 문제점
- 암호화는 **비밀번호 관리에 적합하지 않습니다**. 비밀번호를 암호화하려면 비밀 키가 필요하며, 이 **키를 잃어버릴 경우 데이터의 기밀성이 상실**됩니다.


## 대칭 암호화
- **암호화와 복호화에 동일한 비밀 키를 사용**합니다.
- 대칭 암호화는 키 관리가 어렵고, 키가 유출될 경우 보안이 위협받습니다.
- **휴지 상태인 데이터 시나리오나 단일 애플리케이션 사용만 관련된 경우에도 대칭 암호화를 사용**할 수 있습니다.
- 사용 예시 : 데이터베이스 암호화, 파일 암호화, VPN 연결
![[Spring_Security_37.png]]


## 비대칭 암호화 ✔
- **두 개의 키(공개 키와 비밀 키)를 사용**합니다.
- 공개 키는 누구나 사용할 수 있지만, **비밀 키는 소유자**만 가지고 있습니다.
- 사용 예시: SSL/TLS와 같은 보안 통신 프로토콜, 디지털 서명 및 인증서
![[Spring_Security_38.png]]


# 38. 암호화 및 복호화 데모
평문 비밀번호를 암호화하고, 이를 복호화하여 원래 값을 도출하는 **과정을 시연**하겠습니다.

## 평문 비밀번호
- 평문 비밀번호: `EazyBytes@12345`
- 이 비밀번호는 `plain.txt`라는 파일에 저장되어 있습니다.


## 암호화 과정
- 암호화를 수행하기 위해 OpenSSL을 사용합니다.
- `encrypted.txt` 파일을 열어보면, 암호화된 데이터가 이진 형식으로 저장되어 있습니다.
```bash
openssl enc -aes-256-cbc -pass pass:12345 -pbkdf2 -in plain.txt -out encrypted.txt -base64
```
- **-aes-256-cbc**: 암호화 알고리즘
- **-pass pass:12345**: 비밀 키 (실제 환경에서는 복잡한 키 사용 권장)
- **-pbkdf2**: 비밀번호를 기반으로 솔트, 즉 무작위 값을 추가할 것임.
- **-base64**: 암호화된 데이터를 읽을 수 있는 형태로 인코딩


## 암호화된 데이터 읽기
- 암호화된 데이터를 읽을 수 있는 형식으로 변환하기 위해 base64 인코딩을 사용합니다.
- 결과적으로 `decrypted.txt` 파일에 평문 비밀번호가 복원됩니다.
```bash
openssl enc -aes-256-cbc -pass pass:12345 -d -in encrypted.txt -out decrypted.txt -base64
```
- 동일한 비밀 키와 알고리즘을 사용하여 복호화합니다.


## 비밀번호 확인
- `decrypted.txt` 파일을 열어보면 원래 비밀번호인 `EazyBytes@12345`가 표시됩니다.
- 잘못된 비밀번호를 입력하면 오류가 발생합니다.


# 39. 해싱 소개
비밀번호 관리를 위한 **해싱의 중요성과 해싱 프로세스**를 자세히 살펴보겠습니다.

## 해싱의 필요성 ✔
- 해싱은 비밀번호를 **안전하게 저장하기 위한 최선의 방법**입니다. 평문 비밀번호를 데이터베이스에 저장하지 않고, 해시 값을 저장합니다.
![[Spring_Security_39.png]]


## 해싱의 장점 ✔
- **단방향성**: 해싱된 데이터는 되돌릴 수 없습니다.
- **일관성**: 동일한 입력에 대해 항상 동일한 해시 값을 제공합니다.
- **고정된 출력**: 입력 데이터의 크기에 관계없이 항상 고정된 길이의 해시 값을 생성합니다.
```c
입력: 12345
해시 값 (SHA-256): 5994471abb01112afcc18159f6cc74b4f512b4b9ba2e72c2e2f2b2d3e3f73f3
````


## 해싱 데모
- OpenSSL을 사용하여 해싱을 시연합니다.
```bash
echo -n "EazyBytes@12345" | openssl dgst -sha256
```
- 동일한 입력에 대해 항상 동일한 해시 값을 얻습니다.
- 예를 들어, 긴 문자열을 입력하더라도 출력 길이는 고정됩니다.


## 해싱을 통한 비밀번호 검증 ✔
[Bcrypt-Generator.com - Generate, Check, Hash, Decode Bcrypt Strings](https://bcrypt-generator.com/)
로그인 과정에서 엔드 유저가 **입력한 비밀번호를 검증하기 위해 해싱을 사용**합니다:
1. 사용자가 입력한 일반 텍스트 비밀번호에 해싱을 적용하여 새로운 해시값을 생성합니다.
2. 데이터베이스에 저장된 기존 해시값과 비교합니다.(`matches 함수`)
3. 두 해시값이 일치하면 로그인 성공, 그렇지 않으면 로그인 실패입니다.


## 해싱 vs 암호화 ✔
- **해싱은 데이터를 한 방향으로 변환**하며, **원래 데이터를 복원할 수 없습**니다.
- **암호화는 데이터를 양방향으로 변환**할 수 있으며, **복호화 키가 필요**합니다.


# 40. 해싱의 단점 및 무차별 대입 공격, 사전 공격 또는 레인보우 테이블이란 무엇인지
**해싱의 장점과 단점**에 대해 논의하고, 비밀번호 관리에 해싱이 왜 중요한지를 살펴보겠습니다.


## 해싱의 단점 ✔
1) 동일한 비밀번호는 **동일한 해시 값을 생성**
예를 들어, 여러 사용자가 `12345`라는 비밀번호를 사용할 경우 모두 `aef`라는 해시 값을 가지게 됩니다. 해커가 데이터베이스를 탈취하면, 동일한 해시 값을 가진 사용자들이 같은 비밀번호를 사용하고 있음을 알 수 있습니다.

2) 해시 **속도 빠름**
해시 함수는 매우 빠르기 때문에 해커가 다양한 조합을 시도하는 데 유리합니다. 이로 인해 해커는 짧은 시간 안에 많은 비밀번호를 시도할 수 있습니다.
![[Spring_Security_40 1.png]]

## 무차별 대입 공격 (Brute Force Attack) ✔
- 패스워드에 사용될 수 있는 **문자열의 범위를 정하고 그 범위 내에서 생성 가능한 모든 패스워드를 생성하여 대입한 후 패스워드가 일치 여부를 확인**하는 크래킹 방법이다. 


## 레인보우 테이블 공격 ✔
- **패스워드를 해시 처리하여 패스워드와 해시로 이루어진 체인을 많이 만들어 놓은 테이블을 가지고 대입하여 공격**한다.


# 41. 해싱의 단점, 무차별 대입 공격 및 사전 테이블 공격을 극복하는 방법
해싱의 단점을 극복하여 비밀번호 관리에서 **해싱을 100% 신뢰할 수 있도록 하는 방법**을 알아보겠습니다.

## 해싱의 두 가지 주요 단점
1. 동일한 입력값에 대해 **항상 동일한 해시 값을 제공**합니다.
2. 해싱 **함수가 매우 빠르기** 때문에 무차별 대입 공격에 취약합니다.


## 레인보우 테이블 공격 방지  ✔
[패스워드의 암호화와 저장 - Hash(해시)와 Salt(솔트) (tistory.com)](https://st-lab.tistory.com/100)
- **솔트**: 무작위 값을 생성하여 비밀번호에 추가합니다.
- **동일한 비밀번호라도 서로 다른 솔트 값을 사용하므로 해시 값이 달라집니다.**
- 예를 들어, 사용자1과 사용자2가 `12345`라는 비밀번호를 사용하더라도 각자의 솔트 값이 다르기 때문에 해시 값이 다릅니다.
![[Pasted image 20240928220002.png]]
 - 이를 통해 비밀번호 확인 과정에서 같은 해시를 생성하도록 사용됩니다. 각 사용자가 고유한 해시 함수를 가지므로, 공격자는 대규모 비밀번호 테이블(레인보우 테이블)을 미리 계산할 수 없습니다.


## 무차별 대입 공격 방지 ✔
1) **해싱 알고리즘 느리게 만들기**
비밀번호 해시를 느리게 설계함으로써 공격자가 여러 비밀번호를 시도하는 것을 어렵게 만듭니다. 일반 해싱 과정은 빠르기 때문에 공격자는 많은 수의 일반 비밀번호를 시도할 수 있습니다. 

2) **로그인 횟수 제한하기**
로그인 횟수 제한함으로써 공격자가 여러 비밀번호를 시도하는 것을 불가능하게 만듭니다. 


# 42. 스프링 시큐리티에서 PasswordEncoders 소개
## PasswordEncoder의 작동 방식 ✔
1. 엔드 유저가 자격 증명(유저 네임과 비밀번호)을 **입력**합니다.
2. **유저가 입력한 정보를 `Authentication`으로 리턴**합니다.
3. Spring Security는 **`UserDetailsManager`의 `loadUserByUsername` 메소드를 호출**하여 정보를 불러옵니다.
4. `Authentication`와 데이터베이스에 저장된 비밀번호를 **비교**합니다.
![[Spring_Security_34.png]]


## PasswordEncoder의 역할 ✔
- **BcryptPasswordEncoder**: 사용자가 입력한 **평문 비밀번호를 해시하여 안전하게 저장하는 역할**을 합니다.
- `registerUser()` 메소드에서 `PasswordEncoder`의 `encode()` 메소드를 사용하여 비밀번호를 해시합니다.
```java
String hashPwd = passwordEncoder.encode(customer.getPwd());
customer.setPwd(hashPwd);
````


## PasswordEncoder 메서드 ✔
![[Spring+Security+Zero+to+Master+along+with+JWT,OAUTH2_34.png]]

1. `String encode(CharSequence rawPassword);`
- 사용자가 입력한 일반 비밀번호(`rawPassword`)를 받아 **해시된 비밀번호로 변환**합니다. 해싱 알고리즘을 사용하여 비밀번호를 안전하게 인코딩합니다.
- 비밀번호를 데이터베이스에 저장하기 전에 해싱하여 저장할 때 사용됩니다.

2. `boolean matches(CharSequence rawPassword, String encodedPassword);`
- 사용자가 입력한 일반 비밀번호(`rawPassword`)와 데이터베이스에 저장된 해시된 비밀번호(`encodedPassword`)를 **비교**합니다. 두 비밀번호가 일치하는지 확인합니다.

3. `default boolean upgradeEncoding(String encodedPassword);`
- 해시된 비밀번호(`encodedPassword`)의 **인코딩 방식을 업데이트**해야 하는지 여부를 판단합니다. 기본 구현은 항상 `false`를 반환합니다.
- 향후 더 강력한 해싱 알고리즘으로 전환할 필요가 있을 때 사용됩니다. 이를 통해 기존 비밀번호 해시를 새 알고리즘으로 재해시할 수 있습니다.


# 43. PasswordEncoder 구현 클래스에 대한 심층 탐구

## NoOpPasswordEncoder ✔
- 비밀번호를 해싱하지 않음. **입력된 값을 그대로 반환**하고, 해시 비교 없이 `equals()` 메소드로 확인.
- **테스트 또는 개발 환경에서 사용** 가능. 프로덕션 환경에서는 권장하지 않음.

## BCryptPasswordEncoder ✔
- Spring Security 팀이 **권장하는 해싱 알고리즘**. 해시 값을 생성할 때 솔트를 자동으로 추가하므로, **동일한 비밀번호라도 매번 다른 해시 값을 생성**합니다.
- 로그 라운드를 설정하여 해싱 과정을 느리게 할 수 있음 (기본값 10).

## SCryptPasswordEncoder
- SCrypt는 비밀번호 해싱을 위한 또 다른 알고리즘으로, **메모리 소모가 많은 방식**입니다.
- SCrypt는 **CPU와 메모리 사용량을 조절**할 수 있어, **공격자가 대량의 해시를 병렬로 계산하기 어렵게 만듭니다.**
- SCrypt는 작업 비용, 메모리 비용, 병렬 처리 수를 조정할 수 있어, 보안 수준을 높일 수 있습니다.

## Argon2PasswordEncoder
- Argon2는 비밀번호 **해싱을 위한 최신 알고리즘**으로, 비밀번호 해싱 대회에서 우승한 알고리즘입니다.
- 메모리 사**용량과 작업 비용을 조정**할 수 있어, 공격자가 해시를 계산하는 데 필요한 자원을 증가시킵니다.


# 44. Bcrypt 비밀번호 인코더를 사용한 등록 및 로그인 데모
해싱과 PasswordEncoder을 검증하기 위해 로그인 과정을 살펴보겠습니다.

## PasswordEncoder를 이용한 회원가입 구현✔
1. 비밀번호 입력: 사용자가 비밀번호를 입력합니다.

2. 랜덤 솔트 생성
- 비밀번호를 해시하기 전에, **랜덤한 솔트 값을 생성**합니다. 이 솔트는 매번 다르게 생성됩니다. ex) `$2a$10$88.f6upbBvy0okEa7OfHFu`
- 입력된 비밀번호와 생성된 **솔트를 결합하여 새로운 문자열을 만듭니다**. 예를 들어, `솔트 + 비밀번호` 형태로 결합한 후, 이 문자열을 해시 함수(예: SHA-256 등)에 입력하여 해시 값을 생성합니다.
▶ BCryptPasswordEncoder클래스
```java
@Override  
public String encode(CharSequence rawPassword) {  
    if (rawPassword == null) {  
       throw new IllegalArgumentException("rawPassword cannot be null");  
    }  
    String salt = getSalt(); // 랜덤한 솔트 값을 생성
    return BCrypt.hashpw(rawPassword.toString(), salt);  
}
```

3. controller에서 만든 비즈니스 로직으로 DB에 저장
- 생성된 **해시 값과 함께 사용한 솔트를 데이터베이스에 저장**합니다. 이때, 솔트는 해시 값과 함께 저장되어야 합니다.
```java
@PostMapping("/register")  
public ResponseEntity<String> registerUser(@RequestBody Customer customer) {  
    try {  
            String hashPwd = passwordEncoder.encode(customer.getPwd());  
            customer.setPwd(hashPwd);  
            Customer savedCustomer = customerRepository.save(customer);  
  
            if(savedCustomer.getId()>0) {  
                return ResponseEntity.status(HttpStatus.CREATED).  
                        body("Given user details are successfully registered");  
            } else {  
                return ResponseEntity.status(HttpStatus.BAD_REQUEST).  
                        body("User registration failed");  
            }  
    } catch (Exception ex) {  
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).  
                body("An exception occurred: " + ex.getMessage());  
    }  
}
```


## PasswordEncoder를 이용한 로그인 과정 이해 ✔
![[Spring_Security_43.png]]

1. 아이디, 비밀번호 입력: 사용자가 아이디와 비밀번호를 **입력**합니다.

2. 비밀번호 검증
- 사용자가 나중에 비밀번호를 입력하면, **해당 비밀번호의 솔트를 데이터베이스에서 가져**옵니다. 그 후, **입력된 비밀번호와 솔트를 결합하여 새로운 해시 값을 생성**합니다.
- 저장된 솔트값 : `$2a$10$88.f6upbBvy0okEa7OfHFu`

3. 비교
- 새로 생성된 해시 값과 데이터베이스에 저장된 **해시 값을 비교(matches메소드)** 합니다. 일치하면 비밀번호가 맞는 것이고, 그렇지 않으면 틀린 것입니다. 
- **저장된 비번은 `{인코딩종류}{랜덤솔트값}{생성된해시값}`으로 구성**됩니다. ex) `{bcrypt}$2a$10$88.f6upbBvy0okEa7OfHFuorV29qeK.sVbB9VQ6J6dWM1bW6Qef8m`


## 로그인 과정 디버깅
1. **DaoAuthenticationProvider**의 `authenticate` 메소드를 디버깅하여 비밀번호 검증 과정을 살펴봅니다.
2. `additionalAuthenticationChecks` 메소드에서 비밀번호를 검증합니다.
3. `matches` 메소드를 통해 입력된 비밀번호와 데이터베이스 비밀번호를 비교합니다.
