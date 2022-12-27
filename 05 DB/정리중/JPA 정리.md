## 1. 의존성  및 yml 설정하기
- gradle
```java
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

- yml
```yaml
spring:

##쿼리 및 테이블 생성을 보기 위한 설정
jpa:
show-sql: true
properties:
hibernate:
format_sql: true
generate-ddl: false

##테이블 생성방식을 어떻게 결정할지 설정
hibernate:
ddl-auto: create-drop

##h2를 연결하기 위한 설정
datasource:
driver-class-name: org.h2.Driver
url: jdbc:h2:tcp://localhost/~/test
username: sa

##mysql을 연결하기 위한 설정
datasource:  
url: jdbc:mysql://localhost:3306/book_manager?autoReconnect=true  
username: root  
password: yohan1234  
driver-class-name: com.mysql.jdbc.Driver
```


## 2.Entity(=자바 객체) 만들기

- domain/User
```java
@Id
@GeneratedValue
private Long id;

@NonNull
private String name;

@NonNull
private String email;

private LocalDateTime createdAt

private LocalDateTime updatedAt;
```

- @Entity
DB와 자바 객체를 연결시켜주는 것이 ORM 기술인데, 여기서 자바 객체를 의미하는 것이 Entity
만들 때 PK가 반드시 필요해 id에 @Id 해줬다

## 3. Repository → Entity 저장하기

- repository/UserRepository
```java
public interface UserRepository extends JpaRepository<User, Long>
```
인터페이스를 선언하는 것만으로도 JpaRepository 기능을 사용할 수 있음


## 4. 기능 테스트 해보기
- test/resources/data.sql → SQL로 Entity 생성하기

```sql
call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (1, 'martin', 'martin@fastcampus.com', now(), now());

call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (2, 'dennis', 'dennis@fastcampus.com', now(), now());

call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (3, 'sophia', 'sophia@slowcampus.com', now(), now());

call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (4, 'james', 'james@slowcampus.com', now(), now());

call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (5, 'martin', 'martin@another.com', now(), now());
```

call next value for hibernate_sequence ⇒ 자동 증가가 아니라 직접 증가해주기

- if) 만약에 call next value for hibernate_sequence 가 없으면?

values의 id 값 = 1과 new User()의 id 값 = 1 이 충돌하게 된다


- test../repository/UserRepositoryTest
```java
@Autowired
private UserRepository userRepository;

@Test
void crud() { // create, read, update, delete
userRepository.save(new User()); //java로 entity 생성하기
	userRepository.findAll().forEach(System.out::println); 
```
![[Pasted image 20221227180758.png]]


## 5. 다양한 형태의 출력

1) 정렬 → 이름의 역순으로 정렬하기

```java
List<User> users = userRepository.findAll(Sort.by(Sort.Direction.DESC, "name"));
users.forEach(System.out::println);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%201.png)

---

2) 원하는 db 선택하기

```java
List<User> users = userRepository.findAllById(Lists.newArrayList(1L, 3L, 5L);
users.forEach(System.out::println);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%202.png)

---

3) getOne과 findById의 차이는? (ch02.02 23:00 참고)

[Spring Data JPA 에서 getOne 과 findById 차이점](https://granger.tistory.com/50)

---

4) 데이터 총 개수 조회

```java
Long count = userRepository.count();
System.out.println(count);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%203.png)

---

5)데이터 존재 조회

```java
boolean exists = userRepository.existsById(1L);
System.out.println(exists);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%204.png)

---

6) delete

```java
userRepository.delete(userRepository.findById(1L).orElse(null));
userRepository.delete(userRepository.findById(1L).orElseThrow(() -> new RuntimeException()));
userRepository.delete(userRepository.findById(1L).orElseThrow(RuntimeException::new));
```

delete 메소드 3개 다 같은 역할

[JPA에서 대량의 데이터를 삭제할때 주의해야할 점](https://jojoldu.tistory.com/235)

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%205.png)

---

7)Page 기능 

```java
Page<User> users = userRepository.findAll(PageRequest.of(1, 3));
System.out.println("page : " + users);
System.out.println("totalElements : " + users.getTotalElements());
System.out.println("totalPage : " + users.getTotalPages());
System.out.println("numberOfElements : " + users.getNumberOfElements());
System.out.println("sort : " + users.getSort());
System.out.println("size : " + users.getSize());

users.getContent().forEach(System.out::println);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%206.png)

---

8)ExampleMatcher

```java
ExampleMatcher matcher = ExampleMatcher.matching()
.withIgnorePaths("name")
.withMatcher("email", endsWith());

Example<User> example = Example.of(new User("ma", "fastcompus.com"), matcher);
userRepository.findAll(example).forEach(System.out::println);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%207.png)

```java
User user = new User();
user.setEmail("slow");

ExampleMatcher.matching().withMatcher("email", contains());
Example<User> example1 = Example.of(user, matcher);

userRepository.findAll(example1).forEach(System.out::println);
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%208.png)

plus) JPA가 sql 만들어지는 형태 보고 싶다면?

- application.yml

```yaml
spring:	
	jpa:
	show-sql: true
	properties:
	  hibernate:
		format_sql: true  
```

show-sql: true → 만들어지는 SQL 보여줌

format_sql → 쿼리가 보기 쉬운 형태로 바꿔줌


plus) 어떻게 구현체가 작동하는지 코드로 알아볼려면?(ch02.03 참고)

SimpleJpaReposotory에서 구현한 코드 보기

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%209.png)

## 6. 쿼리 메소드 직접 구현하기

기존에 JpaRepository에서 제공하는 쿼리 말고 사용자가 원하는 쿼리를 직접 등록해보자(ch03 참고)

- repository/UserRepository → 기본실습1

```java
List<User> findByName(String name); //데이터가 2개 이상일 때, List를 하지 않으면 unique result 오류남
Optional<User> findByName(String name);
set<User> findByName(String name);

User findByEmail(String email);
User getByEmail(String email);
User readByEmail(String email);
User queryByEmail(String email);
User searchByEmail(String email);
User streamByEmail(String email);
User findUserByEmail(String email);

List<User> findFirst2ByName(String name); //몇번째 순서꺼인지 정할 수 있어
List<User> findTop2ByName(String name);
List<User> findLast1ByName(String name);
```

직접 만들고자 할 때는 공식 문서를 참고해서 규칙 지키기

---

- repository/UserRepository → 기본실습2

```java
List<User> findByEmailAndName(String email, String name);
List<User> findByEmailOrName(String email, String name);

List<User> findByCreatedAtAfter(LocalDateTime yesterday);
List<User> findByIdAfter(Long id);

List<User> findByCreatedAtGreaterThan(LocalDateTime yesterday);
List<User> findByCreatedAtGreaterThanEqual(LocalDateTime yesterday);
List<User> findByCreatedAtBetween(LocalDateTime yesterday, LocalDateTime tomorrow);

List<User> findByIdBetween(Long id1, Long id2);
List<User> findByIdGreaterThanEqualAndIdLessThanEqual(Long id1, Long id2);
```

---

- repository/UserRepository → 기본실습2(2)

```java
List<User> findByIdIsNotNull();
//List<User> findByAddressIsNotEmpty();  name is not null and name != '' ??

List<User> findByNameIn(List<String> names);
List<User> findByNameStartingWith(String name);
List<User> findByNameEndingWith(String name);
List<User> findByNameContains(String name);
List<User> findByNameLike(String name);

//3개다 같은 의미지만, 가독성을 위해서 사용함
Set<User> findUserByNameIs(String name);
Set<User> findUserByName(String name);
Set<User> findUserByNameEquals(String name);
```

---

- repository/UserRepository → 정렬

```java
List<User> findTop1ByName(String name);
List<User> findTopByNameOrderByIdDesc(String name);
List<User> findFirstByNameOrderByIdDescEmailAsc(String name);
List<User> findFirstByName(String name, Sort sort) //정렬을 파라미터로 넣음
```

- findTop1ByNameOrderByIdDesc

find할거야 top1 위에서 첫번째껄, ByName 이름을 규칙으로

Order 순서는 ById Id를 규칙으로 할거고, Desc 내림차순할거야


---

- repository/UserRepository → 페이징

```java
Page<User> findByName(String name, Pageable pageable);
```

## 7. 연관 관계

### 1) 개별 Entity 출력해보기

- domain/BookReviewInfo

```java
@Entity
@NoArgsConstructor
@Data
@ToString(callSuper = true) //상속받는 것도 출력하기 위함
@EqualsAndHashCode(callSuper = true) //상속받는 것도 출력하기 위함
public class BookReviewInfo extends BaseEntity {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;

private Long bookId;

private float averageReviewScore;

private int reviewCount;
}
```

- 테스트 해보기 → test../repository/BookReviewInfoRepositoryTest

```java
@Autowired
private BookReviewInfoRepository bookReviewInfoRepository;

@Test
void crudTest() {
BookReviewInfo bookReviewInfo = new BookReviewInfo();
bookReviewInfo.setBookId(1L);
bookReviewInfo.setAverageReviewScore(4.5f);
bookReviewInfo.setReviewCount(2);

bookReviewInfoRepository.save(bookReviewInfo);

System.out.println(">>> " + bookReviewInfoRepository.findAll());
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%2010.png)

- domain/Book

```java
@Entity
@NoArgsConstructor
@Data
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
public class Book extends BaseEntity {
@Id
@GeneratedValue
private Long id;

private String name;

private String category;

private Long authorId;

private Long publisherId;
}
```

---

### 2) 단방향 연관 시켜 보기

- /damin/Member

```java
@Id
@GeneratedValue
@Column(name = "MEMBER_ID")
private Long id;

@Column(name = "USERNAME")
private String name;

@Column(name = "TEAM_ID2")
private Long teamId;

@ManyToOne
@JoinColumn(name = "TEAM_ID")
private Team team;

public Member(String name, Long teamId) {
this.name = name;
this.teamId = teamId;
}

public Member(String name, Long teamId, Team team) {
this.name = name;
this.teamId = teamId;
this.team = team;
}

```

- damin/Team

```java
@Id @GeneratedValue
@Column(name = "TEAM_ID")
private Long id;

private String name;

public Team(String name) {
this.name = name;
}
```

- JPA개념 활용 X 조회해서 넣어보기

```java
@Test
void member_test(){
Team team = new Team("TeamA");
teamRepository.save(team);
memberRepository.save(new Member("member1", team.getId()));
memberRepository.save(new Member("member2", team.getId()));

memberRepository.findAll().forEach(System.out::println);
}
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%2011.png)

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%2012.png)

- JPA개념 활용

```java
@Test
void member_test2(){
Team team = new Team("TeamA");
teamRepository.save(team);
memberRepository.save(new Member("member3", team.getId(), team));
memberRepository.save(new Member("member4", team.getId(), team));

memberRepository.findAll().forEach(System.out::println);
}
```

![Untitled](JPA%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20d57bc147d441430f898fdddc6b1cc4eb/Untitled%2013.png)

---

### 3)양방향 연관 관계

- damin/Team 에 추가 시키기

```java
...
@OneToMany(mappedBy = "team")
private List<Member> members = new ArrayList<>();
...
```