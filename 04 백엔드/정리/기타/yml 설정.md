#yml


----

## 서버 설정
```yaml
server:
  port: 8080
```
- `port`: 애플리케이션이 실행될 포트를 지정합니다.


## 데이터베이스 설정
```yaml
spring:
  datasource:
	url: jdbc:mysql://localhost:3306/mydb
	username: root
	password: password
	driver-class-name: com.mysql.cj.jdbc.Driver
```
- `url`: 데이터베이스 URL
- `username`: 데이터베이스 사용자 이름
- `password`: 데이터베이스 비밀번호
- `driver-class-name`: JDBC 드라이버 클래스 이름


## JPA 설정
▶ 기본 설정 
```yaml
spring:
  jpa:
	hibernate:
	  ddl-auto: update
	show-sql: true
	properties:
	  hibernate:
		format_sql: true
```
- `ddl-auto`: 스키마 자동 생성 전략 (예: `none`, `validate`, `update`, `create`, `create-drop`)
- `show-sql`: 실행되는 SQL을 콘솔에 출력할지 여부
- `format_sql`: 출력되는 SQL을 포맷할지 여부

▶ 고급 설정
```yaml
spring:
  jpa:
    open-in-view: true
    generate-ddl: true
    database-platform: org.hibernate.dialect.MySQL5Dialect
    properties:
      hibernate:
        cache:
          use_second_level_cache: true
          use_query_cache: false
        hbm2ddl:
          auto: update
```

1. `open-in-view`
- 기본값은 `true`로, 이는 Spring MVC가 요청을 처리하는 동안 영속성 컨텍스트를 열어 둡니다. 이는 Lazy Loading을 지원하기 위해 사용됩니다.

2. `generate-ddl`
- Hibernate가 데이터베이스 스키마를 자동으로 생성할지 여부를 결정합니다.

3. `database-platform`
- 사용할 데이터베이스 방언(dialect)을 지정합니다. 예를 들어, MySQL을 사용할 경우 `org.hibernate.dialect.MySQL5Dialect`를 지정합니다.

4. 2차 캐시 설정:
- `use_second_level_cache`: 2차 캐시 사용 여부를 설정합니다.
- `use_query_cache`: 쿼리 캐시 사용 여부를 설정합니다.


## 로깅 설정
```yaml
logging:
  level:
	root: INFO
	com.example: DEBUG
  file:
	name: application.log
```
- `level`: 로깅 레벨을 설정합니다. (예: `INFO`, `DEBUG`, `ERROR`)
- `file.name`: 로그 파일의 이름을 지정합니다.


## 프로파일 설정
```yaml
spring:
  profiles:
	active: dev
```
- `active`: 활성화할 프로파일을 지정합니다. (예: `dev`, `prod`)


## 메일 설정
```yaml
spring:
  mail:
	host: smtp.example.com
	port: 587
	username: myemail@example.com
	password: password
	properties:
	  mail:
		smtp:
		  auth: true
		  starttls:
			enable: true
```
- `host`: SMTP 서버 호스트
- `port`: SMTP 서버 포트
- `username`: 이메일 사용자 이름
- `password`: 이메일 비밀번호
- `properties.mail.smtp.auth`: SMTP 인증 사용 여부
- `properties.mail.smtp.starttls.enable`: TLS 사용 여부