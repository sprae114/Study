# 01 JPA 소개
### 왜 JPA를 사용해야 하는가?
#### 📌생산성
-   JPA를 자바 컬렉션에 객체를 저장하듯 JPA에게 저장할 객체를 전달.
-   INSERT SQL을 작성하고 JDBC API 사용하는 지루하고 반복적인 일을 JPA가 대신 처리해준다.
-   CREATE TABLE같은 DDL문 자동 생성
-   데이터베이스 설계 중심의 패러다임을 객체 설계 중심으로 역전
```java
jpa.persist(member);    // 저장
Member member = jpa.find(memberId);     // 조회
```


#### 📌유지 보수
-   엔티티에 필드 추가시 등록, 수정, 조회 관련 코드 모두 변경
-   JPA를 사용하면 이런 과정을 JPA가 대신 처리
-   개발자가 작성해야 할 SQL과 JDBC API 코드를 JPA가 대신 처리해줌으로 유지보수해야 하는 코드 수가 줄어든다.


#### 📌패러다임 불일치 해결
상속, 연관관계, 객체 그래프 탐색, 비교하기 같은 패러다임 불일치 해결


#### 📌성능
```java
String memberId = "helloId"
Member member1 = jpa.find(memberId);
Member member2 = jpa.find(memberId);
```
- JDBC API를 사용해서 작성하면 조회할때 마다 SELECT SQL을 사용해서 DB와 두 번 통신했을 것이다.  그러나 JPA를 사용하면 회원을 조회하는 SELECT SQL을 한 번만 데이터베이스에 전달하고 두 번째는 조회한 회원 객체를 재사용한다.
-   다양한 성능 최적화 기회 제공
-   어플리케이션과 데이터베이스 사이에 존재함으로 여러 최적화 시도 가능

#### 📌데이터 접근 추상화와 벤더 독립성
-   데이터베이스 기술에 종속되지 않음 데이터베이스를 변경하면 JPA에게 다른 데이터베이스를 사용한다고 알려주면 됨.
![[Pasted image 20221126163707.png]]

## JPA가 뭐야?
#### 📌JPA (Java Persistence API)
- 자바 진영의 ORM 기술 표준
- ORM이란? 객체와 RDMS를 매핑한다는 의미

#### 📌JPA를 사용해서 어떻게 달라진거야?
- 기존 JAVA와 DB 동작방식
![[Pasted image 20221126164150.png]]

- JPA를 사용한 후 동작방식
![[Pasted image 20221126164232.png]]

예시) 객체를 저장하는 코드
- JDBC를 이용하는 경우
![[Pasted image 20221126164406.png]]

- JPA를 이용하는 경우
```java
jpa.persist(member) //저장
```
![[Pasted image 20221126164514.png]]

# 02 JPA 시작
## 전체 코드
```java
public static void main(String[] args) {  
  
    //엔티티 매니저 팩토리 생성  
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");  
    EntityManager em = emf.createEntityManager(); //엔티티 매니저 생성  
  
    EntityTransaction tx = em.getTransaction(); //트랜잭션 기능 획득  
  
    try {  
  
  
        tx.begin(); //트랜잭션 시작  
        logic(em);  //비즈니스 로직  
        tx.commit();//트랜잭션 커밋  
  
    } catch (Exception e) {  
        e.printStackTrace();  
        tx.rollback(); //트랜잭션 롤백  
    } finally {  
        em.close(); //엔티티 매니저 종료  
    }  
  
    emf.close(); //엔티티 매니저 팩토리 종료  
}  
  
public static void logic(EntityManager em) {  
  
    String id = "id1";  
    Member member = new Member();  
    member.setId(id);  
    member.setUsername("지한");  
    member.setAge(2);  
  
    //등록  
    em.persist(member);  
  
    //수정  
    member.setAge(20);  
  
    //한 건 조회  
    Member findMember = em.find(Member.class, id);  
    System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());  
  
    //목록 조회  
    List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();  
    System.out.println("members.size=" + members.size());  
  
    //삭제  
    em.remove(member);  
  
}
```

출력
```
findMember=지한, age=20
memberss.size=1
```

## 엔티티 매니저 설정
#### 📌JPA를 사용하기 위한 설정
![[Pasted image 20221126183229.png]]
1) 설정 정보 조회
2) 엔티티 매니저 팩토리 생성
3) 엔티티 매니저 생성

#### 📌 엔티티 매니저 팩토리의 역할은?
- 설정 정보를 읽어서 JPA를 동작시키기 위한 객체만들기
- 데이터베이스 커넥션 풀 생성하기
- 생성비용이 크므로,  딱 한 번만 생성하고 공유해서 사용함.

#### 📌 엔티티 매니저의 역할은?
- 내부에 DB 커넥션을 유지하면서 데이터 베이스와 통신함.
- 엔티티를 데이터베이스에 등록/수정/삭제/조회할 수 있음.
- 커넥션과 밀접한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안됌.

## 트랜잭션 관리
#### 📌 트랜잭션을 사용해야되는 이유는?
- JPA는 트랜잭션 없이 데이터를 변경하면 예외발생함.

```java
EntityTransaction tx = em.getTransaction(); //트랜잭션 기능 획득  
  
try {  
    tx.begin(); //트랜잭션 시작  
    logic(em);  //비즈니스 로직  
    tx.commit();//트랜잭션 커밋  
  
} catch (Exception e) {  
    e.printStackTrace();  
    tx.rollback(); //트랜잭션 롤백
}
```

## 비즈니스 로직

```java
public static void logic(EntityManager em) {  
  
    String id = "id1";  
    Member member = new Member();  
    member.setId(id);  
    member.setUsername("지한");  
    member.setAge(2);  
  
    //등록  
    em.persist(member);  
  
    //수정  
    member.setAge(20);  
  
    //한 건 조회  
    Member findMember = em.find(Member.class, id);  
    System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());  
  
    //목록 조회  
    List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();  
    System.out.println("members.size=" + members.size());  
  
    //삭제  
    em.remove(member);  
  
}
```

#### 📌어떻게 비즈니스 로직이 실행되는 거지?
- 비즈니스 로직은 엔티티 매니저를 통해서 수행됌.

#### 📌 수정에서 주의점
- 엔티티를 수정한 후 엔티티 매니저를 통해 값을 반영해야 될거 같은데, 엔티티값만 변경됌.
- why? JPA는 어떤 엔티티가 변경되었는지 추적하는 기능을 갖추고 있음.

#### 📌 한건 조회
- find(조회할 엔티티 타입, 기본키 or 식별자 값)을 사용해서 조회함

#### 📌 목록 조회의 문제점
- 문제점 : 테이블이 아닌 엔티티 객체를 대상으로 검색하려면 데이터베이스 모든 데이터를 어플리케이션으로 불러와 엔티티 객체로 변경해서 검색해야 함.
- 해결책 : JPQL사용
ex) em.createQuery(JPQL, 반환타입) -> 쿼리객체 생성하기

#### 📌 JPQL과 SQL차이는?
- JPQL : 엔티티 객체를 대상으로 쿼리. 즉, 클래스와 필드 대상으로 쿼리함.
- SQL : 데이터베이스 테이블을 대상으로 쿼리

# 03 영속성 관리
### EntityManger 생성
#### 📌 엔티티 매니저 팩토리 동작방식
![[Pasted image 20221126223820.png]]
- 엔티티 매니저 팩토리를 생성할때 커넥션풀도 같이 만든다.
- DB 연결이 필요한 시점까지는 커넥션을 얻지 X

## 영속성 컨텍스트
#### 📌 영속성 컨텍스트
- 엔티티를 영구 저장하는 환경
- em.persist(member) -> 정확히는 엔티티 매니저를 사용해서 회원 엔티티를 영속성 컨텍스트에 저장

#### 📌 영속성 컨텍스트의 특징
1) 영속성 컨텍스트와 식별자 값
- 엔티티를 식별자 값(@id로 테이블의 기본 키와 매핑한 값)으로 구분
- 영속 상태는 식별자 값이 반드시 있어야 한다. 식별자 값이 없으면 예외 발생.

2)  영속성 컨텍스트와 데이터베이스 저장
- JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영
-  플러시(flush)

3)  영속성 컨텍스트가 엔티티를 관리하는 것의 장점
-   1차 캐시 -> 밑에서 설명
-   동일성 보장
```java
Member a = em.find(Member.class, "member1"); 
Member b = em.find(Member.class, "member1"); 

System.out.println(a == b); // 동일성 비교
```
-   트랜잭션을 지원하는 쓰기 지연
-   변경 감지
-   지연 로딩

#### 📌 엔티티의 생명주기
- 그림으로 보는 생명주기
![[Pasted image 20221126225038.png]]

1) 비영속(new/transient) 
- 영속성 컨텍스트와 전혀 관계가 없는 상태
- 순수한 객체 상태로, 영속성 컨텍스트나 데이터베이스와 상관없음
```java
// 객체를 생성한 상태(비영속) 
Member member = new Member(); 
member.setId(100L); 
member.setUsername("회원1");
```
![[Pasted image 20221126225357.png]]

2) 영속(managed)
- 엔티티 매니저를 통해 엔티티를 영속성 컨텍스트에 저장상태.
- 영속성 컨텍스트는 내부에 캐시를 가지고 있음
- 1차 캐시에 member가 key : value 쌍으로 저장
```java
// 엔티티를 생성한 상태(비영속) 
Member member = new Member(); 
member.setId(100L); 
member.setUsername("회원1"); // 엔티티 영속 
em.persist(member);
```
![[Pasted image 20221126233938.png]]

3) 준영속(detached)
- 영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 "준영속 상태".
- 언제 준영속상태가되지?
	-   em.detach() 호출로 준영속 상태 명시적 호출.
	-   em.close()를 호출해서 영속성 컨텍스트를 닫음.
	-   em.clear로 영속성 컨텍스트 초기화
```java
// 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태 
em.detach(member);
```

4) 삭제(removed)
- 엔티티를 영속성 컨텍스트와 데이타베이스에서 삭제.
```java
// 객체를 삭제한 상태(삭제) 
em.remove
```


#### 📌 비영속과 준영속의 차이는?
- 식별자의 유무가 있는냐에 차이임. 영속상태가 되려면 식별자가 반드시 존재해야 합니다. 그래서 영속 상태가 되었다가 다시 준영속 상태가 되면 식별자가 항상 존재하게 됩니다.

출처 : [준영속 상태와 비영속 상태의 차이점이 있을까요?](https://www.inflearn.com/questions/45195)


#### 📌 엔티티 조회
- em.find() 호출 : 1차 캐시에서 엔티티 조회 엔티티가 1차 캐시에 없으면 데이터베이스 조회

1) 1차 캐시에서 조회
- 찾는 엔티티가  있으면 DB에 조회하지 않고 메모리에 있는 1차 캐시에서 엔티티를 조회함.
```java
Member member = em.find(Member.class, 100L);
```
![[Pasted image 20221126233809.png]]	

2) 데이터베이스에서 조회
- 엔티티가 1차 캐시에 없으면 엔티티 매니저는 데이터베이스를 조회해서 엔티티를 생성. 
- 1차 캐시에 저장한 후에 영속 상태의 엔티티를 반환.
![[Pasted image 20221126234402.png]]

