#JPAConfig , #Config

----
# (코드) JPA 설정
▶build.gradle
```java
// JDBC를 주석처리하고 JPA, 스프링 데이터 JPA 추가
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```


# Config 설정
```java
@Configuration 
@ComponentScan("com.sp.fc.paper") 
@EnableJpaRepositories(basePackages = { "com.sp.fc.paper.repository" }) @EntityScan(basePackages = { "com.sp.fc.paper.domain" }) 
@EnableJpaAuditing
public class PaperModule {

}
```

#### @Configuration
클래스가 **Spring IoC 컨테이너에 의해 빈 설정**을 위한 것임을 나타냅니다. 이 클래스에서는 빈을 정의하거나 다른 설정 클래스를 가져오는 등의 작업을 수행할 수 있습니다.


#### @ComponentScan
키워드 : 일반적인 빈

`@ComponentScan("com.sp.fc.paper")`
"com.sp.fc.paper" 패키지와 그 하위 패키지에서 **컴포넌트를 찾도록 Spring에 지시**합니다. 이 컴포넌트는 `@Component`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션이 붙은 클래스일 수 있습니다.


#### @EnableJpaRepositories
키워드 : JPA 리포지토리

`@EnableJpaRepositories(basePackages = {"com.sp.fc.paper.repository"})`
"com.sp.fc.paper.repository" 패키지와 그 하위 패키지에 있는 **Spring Data JPA 리포지토리 인터페이스를 찾도록 Spring에 지시**합니다. 이를 통해 데이터 접근 계층의 코드를 간소화할 수 있습니다.

`@ComponentScan`만 사용하면 Spring Data JPA의 Repository 인터페이스를 정확하게 인식하지 못합니다.


#### @EntityScan
키워드 : JPA 엔티티

`@EntityScan(basePackages = {"com.sp.fc.paper.domain"})`
"com.sp.fc.paper.domain" 패키지와 그 하위 패키지에 있는 JPA 엔티티를 찾도록 Spring에 지시합니다. **JPA 엔티티 클래스를 찾아서 엔티티 매니저에 등록하는 데 사용**되므로, 각각 다른 역할을 수행합니다.


# 초기 데이터 셋팅
## @GeneratedValue를 사용하지 않으면..
다음 sql문을 증가시키지 않으면 sequence값이 증가하지 않기 때문에 충돌발생함.
```sql
call next value for hibernate_sequence;
```

`@GeneratedValue` 애노테이션을 사용하지 않았다면, 다음과 같이 해야함.
```sql
call next value for hibernate_sequence;
insert into user (`id`, `name`, `email`, `created_at`, `updated_at`) values (1, 'martin', 'martin@fastcampus.com', now(), now());

call next value for hibernate_sequence;
insert into user ....

```

- `@GeneratedValue(strategy = GenerationType.IDENTITY)`일때, id값은 넣지 않음.
```sql
insert into user (name, email, created_at, updated_at) values ('Timbs', 'ztimbs0@1und1.de', '2022-09-05 10:09:55', '2023-03-06 23:08:47');
```

## data.sql을 사용하기 위한 세팅
[[Spring-error해결] data.sql을 만들 때 에러 (velog.io)](https://velog.io/@99winnmin/Spring-error%ED%95%B4%EA%B2%B0-data.sql%EC%9D%84-%EB%A7%8C%EB%93%A4-%EB%95%8C-%EC%97%90%EB%9F%AC)
[[JPA] SpringBoot 2.5 이후부터 data.sql 초기화 시점 (tistory.com)](https://blogshine.tistory.com/437)
[How to use Hibernate identifier sequence generators properly](https://ntsim.uk/posts/how-to-use-hibernate-identifier-sequence-generators-properly)


application.yml에 다음 설정추가
```yaml
spring:
	jpa:
    	defer-datasource-initialization: true

sql:  
  init:  
    mode: always  
    ## 밑에는 상황에 맞게 선택해서 사용하자
    schema-locations: data.sql  
    data-locations: data.sql
    data-locations: initSetting/*.sql
```
1. `spring.jpa.defer-datasource-initialization: true`
애플리케이션 시작 시점에 **데이터 소스의 초기화를 지연시키는 설정**입니다. `true`로 설정하면, Hibernate가 스키마를 생성한 후에 데이터 소스가 초기화됩니다. 이를 통해 JPA 엔티티 기반의 SQL 스크립트 실행 시, 해당 엔티티가 아직 생성되지 않은 상태에서 스크립트 실행을 방지할 수 있습니다.

2. `sql.init.mode: always`
SQL **초기화 모드를 설정**합니다. `always`로 설정하면, 애플리케이션을 시작할 때마다 SQL 초기화 스크립트가 실행됩니다.

3. `sql.init.schema-locations: data.sql`
**스키마 초기화 스크립트의 위치를 설정**합니다. 여기서 "스키마 초기화"는 데이터베이스의 테이블 구조를 생성하거나 변경하는 작업을 의미합니다. 스크립트에는 `CREATE TABLE`, `ALTER TABLE` 등의 **DDL 명령**이 포함될 수 있습니다.

4. `sql.init.data-locations: data.sql, initSetting/*.sql`
**데이터 초기화 스크립트의 위치를 설정**합니다. 여기서 "데이터 적재"는 데이터베이스의 **테이블에 데이터를 입력하는 작업**을 의미합니다. `INSERT INTO` 등의 **DML명령**이 포함될 수 있습니다.
`data.sql` 파일과 `initSetting` 디렉토리 아래의 모든 `.sql` 파일을 데이터 초기화 스크립트로 사용하도록 설정하였습니다