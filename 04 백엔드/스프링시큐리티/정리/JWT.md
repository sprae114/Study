[[Spring] JWT Refresh Token을 사용한 로그인과 고찰 :: 프로찍먹러 (tistory.com)](https://llshl.tistory.com/32)
[[JWT Refresh Token을 이용한 로그인 ](https://tae-jun.tistory.com/19)
[[Server] JWT(Json Web Token)란? - MangKyu's Diary (tistory.com)](https://mangkyu.tistory.com/56)
[[Spring Boot]JWT 토큰 생성, 검증, 정보 추출 (velog.io)](https://velog.io/@qazws78941/Spring-BootSecurity-JWT%EB%A1%9C-%EC%9D%B8%EC%A6%9D-%EB%B0%8F-%EC%9D%B8%EA%B0%80-1)
[🌐 Access Token & Refresh Token 원리 (tistory.com)](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT)

----
# 이론
## JWT란?
JWT(Json Web Token)란 **Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token**이다. 주로 회원 인증이나 정보 전달에 사용되는 JWT는 아래의 로직을 따라서 처리된다.


## 토큰 기반 인증 시스템이란?
토큰 기반의 인증 시스템은 인증받은 사용자들에게 토큰을 발급하고, 서버에 요청을 할 때 헤더에 토큰을 함께 보내도록 하여 유효성 검사를 한다. 이러한 시스템에서는 더이상 사용자의 인증 정보를 서버나 세션에 유지하지 않고 클라이언트 측에서 들어오는 요청만으로 작업을 처리한다. 즉, 서버 기반의 인증 시스템과 달리 상태를 유지하지 않으므로 Stateless한 구조를 갖는다. 이러한 토큰 기반의 인증 방식을 통해 수많은 문제점들을 해결할 수 있는데, 대표적으로 사용자가 로그인이 되어있는지 안되어있는지 신경쓰지 않고 손쉽게 시스템을 확장할 수 있다.

![[Pasted image 20240306194827.png]]

1. 사용자가 아이디와 비밀번호로 **로그인**을 한다.
2. 서버 측에서 해당 **정보를 검증**한다.
3. 정보가 정확하다면 서버 측에서 사용자에게 **Signed 토큰을 발급**한다. (Signed는 해당 토큰이 서버에서 정상적으로 발급된 토큰임을 증명하는 Signature를 가지고 있다는 것)
4. **클라이언트 측에서 전달받은 토큰을 저장**해두고, 서버에 요청을 할 때마다 해당 토큰을 서버에 함께 전달한다. 이때 **Http 요청 헤더에 토큰을 포함**시킨다.
5. **서버는 토큰을 검증하고, 요청에 응답**한다.


## JWT 구조
JWT는 Header, Payload, Signature의 3 부분으로 이루어지며, Json 형태인 각 부분은 Base64Url로 인코딩 되어 표현된다. 또한 각각의 부분을 이어 주기 위해 . 구분자를 사용하여 구분한다.
![[Pasted image 20240306192327.png]]

▶ Header
- typ: 토큰의 타입을 지정 ex) JWT
- alg: 알고리즘 방식을 지정하며, 서명(Signature) 및 토큰 검증에 사용 ex) HS256(SHA256) 또는 RSA

▶ Payload
토큰의 페이로드에는 토큰에서 사용할 정보의 조각들인 **클레임(Claim)** 이 담겨 있다. 


▶ Signature
서명(Signature)은 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드이다. 


## JWT를 쓰는 이유는?
1. **상태를 유지하지 않는다(Stateless)**
JWT는 상태를 저장하지 않는 RESTful 서비스에 적합합니다. 서버는 클라이언트의 상태를 추적하거나 저장할 필요 없이, 매 요청마다 토큰을 디코딩하면 사용자를 인증할 수 있습니다. 이는 서버의 부하를 줄이고 확장성을 향상시킵니다.

2. **보안성**
JWT는 디지털 서명 기술을 사용하여 토큰의 무결성을 보장합니다. 서명은 비밀 키를 사용하여 생성되므로, 토큰이 중간에 변조되었는지 검증할 수 있습니다.

3. **자체 포함적**
JWT는 필요한 모든 정보를 자체적으로 가지고 있습니다. JWT에는 토큰의 발행자, 만료 시간, 클레임 등이 포함되어 있습니다. 이렇게 하면 JWT만으로 필요한 모든 정보를 전달할 수 있습니다.


## JWT를 이용한 로그인 과정
![[Pasted image 20240304201116.png]]
1. 사용자가 id, pw를 입력한다.  
2. 클라이언트는 사용자의 id와 pw를 서버에 POST한다.  
3. 서버는 회원 DB를 통해 사용자 id와 pw가 일치한지 확인한 후 accessToken과 refreshToken을 발급한다.  
4. 토큰 DB에 사용자 id(FK, PK), refresh Token을 저장한다.  
5. 생성된 accessToken과 refreshToken을 클라이언트에 반환한다.

## accessToken 재발급 과정
![[Pasted image 20240304201139.png]]
1. 클라이언트에서 재발급 요청을 보낸다. 이때 헤더에 accessToken과 refreshToken을 담아서 보낸다.  
2. 서버는 받은 accessToken 값을 검증한다. 이때 accessToken값이 조작되거나 분실되어 decode값이 존재하지 않는다면 권한이 없다는 에러 메세지를 클라이언트에게 보낸다.  
3. 클라이언트에게 받은 refreshToken값을 검증한다.  
4. 새로운 accessToken을 발급될 조건이라면 새로운 accessToken을 발급한다.(조건은 아래에서 설명한다.)  
5. 발급된 accessToken을 클라이언트에게 전송한다.


# 실습

## 한글로 과정정리
1. 토큰 객체
2. access token 로직 만들기
```
✔생성 
리플레쉬 토큰 생성 및 만료 확인 후 액세스 토큰 생성

✔조회 
액세스 토큰 만료 날짜 확인
액세스 토큰 정보 조회(이메일, 이름 ..)

✔삭제
액세스 토큰 삭제

```
3. Refresh token 로직 만들기
```
✔생성 
리플레쉬 토큰 없으면 생성 후, 저장?

✔조회
리플레쉬 토큰 만료 날짜 조회

✔삭제
리플레쉬 토큰 삭제
```

