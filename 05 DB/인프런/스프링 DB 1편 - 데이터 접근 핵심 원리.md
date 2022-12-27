# JDBC
## H2 설정
#### 📌<실습> Intelij와 연결하기
- 의존성 추가하기(build.gradle)
```
dependencies {
	...
	runtimeOnly 'com.h2database:h2'
	...
}
```


#### 📌<실습> H2 데이터베이스 서버 실행
- 파일 생성 방법
jdbc:h2:~/test (최초 한번) 이후부터는 jdbc:h2:tcp://localhost/~/test 이렇게 접속


#### 📌<실습> 테이블 생성하기
``` SQL
drop table member if exists cascade;

create table member (
	 member_id varchar(10),
	 money integer not null default 0,
	 primary key (member_id)
);

insert into member(member_id, money) values ('hi1',10000);
insert into member(member_id, money) values ('hi2',20000);
```


## JDBC 이해
#### 📌데이터 베이스를 쓰는 이유는?  
- 애플리케이션을 개발할 때 중요한 **데이터는 대부분 데이터베이스에 보관**함. 자바코드에 보관하면, 자바가 종료되면 데이터가 날라감.

#### 📌서버와 DB 사용법은?
- 동작 과정 그림으로 이해
![[Pasted image 20221130152811.png]]
- 매커니즘  
    - 1. **커넥션 연결**: 주로 TCP/IP를 사용해서 커넥션을 연결한다.  
    - 2. **SQL 전달**: 애플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.  
    - 3. **결과 응답**: DB는 전달된 SQL을 수행하고 그 결과를 응답한다. 애플리케이션 서버는 응답 결과를 활용한다.


#### 📌JDBC 역할은?
- 데이터베이스와 자바를 **연결하는 매개체**가 JDBC API.


#### 📌JDBC의 2가지 문제점
- 1) 다른 종류의 데이터베이스로 **변경하면** 애플리케이션 서버에 개발된 데이터베이스 사용코드도 함께 변경해야 한다.  
- 2) 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 그리고 그 결과를 응답 받는 방법을 새로 학습해야 한다.  즉, 데이터 베이스마다 **통합 X**


#### 📌JDBC 표준 인터페이스의 해결방법은?  
- 첫번째 문제 해결을 위해, 애플리케이션 로직은 이제 **JDBC 표준 인터페이스**에만 의존한다.  따라서 데이터베이스를 다른 종류의 데이터베이스로 변경하고 싶으면 JDBC 구현 라이브러리만 변경하면 된다.
- 두번째 문제 해결을 위해, JDBC **표준 인터페이스 사용법**만 학습하면 된다.  한번 배워두면 수십개의 데이터베이스에 모두 동일하게 적용할 수 있다.


#### 📌JDBC 표준 인터페이스 동작 방법은?
- 인터페이스 구현
![[Pasted image 20221130152951.png]]
- 기존 방식과 연결하는 방법은 동일함.
![[Pasted image 20221130153011.png]]


## JDBC와 최신 데이터 접근 기술
#### 📌최신 기술을 왜 써야할까?  
- 작성해야될 코드가 너무 많고 반복적이라서 사용하기에 **복잡함**
- JDBC 사용하는 경우 로직
    ![[Pasted image 20221130153045.png]]


#### 📌SQL Mapper
- 로직은? 객체와 관계형 데이터베이스의 데이터를 **개발자가 작성한 SQL로 매핑시켜주는 프레임워크**
![[Pasted image 20221130153129.png]]
- 장점  
    - SQL 응답 결과를 **객체로 편리하게 변환**해준다.  
    - JDBC의 **반복 코드를 제거**해준다.  
-   단점  
    - 개발자가 **SQL을 직접 작성**해야한다.


#### 📌ORM 기술  
- 로직은? 객체와 관계형 데이터베이스의 데이터를 **자동으로 매핑**시켜주는 프레임워크
![[Pasted image 20221130153226.png]]
- 장점  
    - 재사용 **유지 보수 편리성이 증가**한다.  
    - 객체 지향적인 코드로 **직관적인 비지니스 로직**을 작성할 수 있다.  
- 단점  
	- 초기 적용에 러닝 커브가 어느 정도 있다.  
	- **복잡한 SQL**의 사용하지 못할 수 도 있다.


#### 📌<실습> H2 데이터베이스 연결
1) 접속 기본정보 기입
접속하는데 필요한 기본 정보를 편리하게 사용할 수 있도록 상수만들기.
```java
public abstract class ConnectionConst {  
    public static final String URL = "jdbc:h2:tcp://localhost/~/test";  
    public static final String USERNAME = "sa";  
    public static final String PASSWORD = "";  
}
```


2) 데이터베이스에 연결 코드
JDBC를 사용해서 실제 데이터베이스에 연결하는 코드 만들기. 
H2 데이터베이스 드라이버가 작동해서 실제 데이터베이스와 커넥션을 맺고 그 결과를 반환해준다. (자동으로 데이터베이스 찾음)
```java
@Slf4j  
public class DBConnectionUtil {  
    public static Connection getConnection() {  
        try {  
            Connection connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);  
            log.info("get connection={}, class={}", connection, connection.getClass());  
            return connection;  
        } catch (SQLException e) {  
            throw new IllegalStateException(e);  
        }  
    }  
}
```


#### 📌JDBC DriverManager 연결 이해
![[Pasted image 20221130160330.png]]
- 매커니즘
	- 1. 애플리케이션 로직에서 **커넥션을 요청**하면 DriverManager. getConnection() 을 호출  why? 해당 데이터 베이스 연결하기 위함
	- 2. DriverManager 는 라이브러리에 등록된 드라이버 **목록을 자동으로 인식**한다. 이 드라이버들에게 순서대로 다음 정보(URL, ID, PASSWORD)를 넘겨서 **커넥션을 획득할 수 있는지 확인**.  
	- 3. 구현체가 클라이언트에 **찾은 커넥션 반환**.


## JDBC 개발
#### 📌<실습> 로직 만들기전에 작성할 것들
- SQL
테이블과 샘플 데이터 만들기를 통해서 member 테이블을 미리 만들어두어야 한다.
```sql
drop table member if exists cascade;
create table member (
	 member_id varchar(10),
	 money integer not null default 0,
	 primary key (member_id)
);
```

- Member : 회원 객체 만들기
```java
@Data  
public class Member {  
  
    private String memberId;  
    private int money;  
  
    public Member() {  
    }  
  
    public Member(String memberId, int money) {  
        this.memberId = memberId;  
        this.money = money;  
    }  
}
```


#### 📌<실습> 등록 로직 
- MemberRepositoryV0 : 회원 객체를 데이터 베이스에 등록. (이해만)
```java
public Member save(Member member) throws SQLException {  
    String sql = "insert into member(member_id, money) values (?, ?)";  
  
    Connection con = null;  
    PreparedStatement pstmt = null;  
  
    try {  
        con = getConnection();  
        pstmt = con.prepareStatement(sql);  
        pstmt.setString(1, member.getMemberId());  
        pstmt.setInt(2, member.getMoney());  
        pstmt.executeUpdate();  
        return member;  
  
    } catch (SQLException e) {  
        log.error("db error", e);  
        throw e;  
  
    } finally {  
        close(con, pstmt, null);  
    }  
  
}

private void close(Connection con, Statement stmt, ResultSet rs) {  
  
    if (rs != null) {  
        try {  
            rs.close();  
        } catch (SQLException e) {  
            log.info("error", e);  
        }  
    }  
  
    if (stmt != null) {  
        try {  
            stmt.close();  
        } catch (SQLException e) {  
            log.info("error", e);  
        }  
    }  
  
    if (con != null) {  
        try {  
            con.close();  
        } catch (SQLException e) {  
            log.info("error", e);  
        }  
    }  
}
```

#### 📌등록 매커니즘 설명  
1) 전달할 **SQL 정의**함  
2) 2가지 준비하기  
	- 데이터베이스에 **커넥션 준비**. (Connetion)과 전달할 SQL  
	- **PreparedStatement** -> (1) DB에 전달할 SQL과 (2)파라미터로 전달할 데이터들을 준비함.  
3) 커넥션 연결은 con, 파라미터 전달은 pstmt를 통해 **바인딩**함.  
	- 바인딩이란 오브젝트의 프로퍼티(메서드 set)에 값을 넣는 것을 말한다.  
4) SQL을 커넥션을 통해 실제 데이터베이스에 **전달**한다.  
5) **리소스 정리 역순**으로 하기


#### 📌<실습> 조회 로직
- MemberRepositoryV0 : 회원 객체를 데이터 베이스에 조회. (이해만)
```java
public Member findById(String memberId) throws SQLException {  
    String sql = "select * from member where member_id = ?";  
  
    Connection con = null;  
    PreparedStatement pstmt = null;  
    ResultSet rs = null;  
  
    try {  
        con = getConnection();  
        pstmt = con.prepareStatement(sql);  
        pstmt.setString(1, memberId);   //찾을 memberId를 pstmt에 저장  

		//조회할때 사용하는 명령문으로 결과를 반환함.  
        rs = pstmt.executeQuery();  
        if (rs.next()) {        //next가 알아서 루프를 돌려주는 군. +  
            Member member = new Member();  
            member.setMemberId(rs.getString("member_id"));  
            member.setMoney(rs.getInt("money"));  
            return member;  
        } else {  
            throw new NoSuchElementException("member not found memberId=" + memberId);  
        }  
    } catch (SQLException e) {  
        log.error("db error", e);  
        throw e;  
    } finally {  
        close(con, pstmt, rs);  
    }  
}
```


#### 📌조회 매커니즘 설명  
1) 전달할 **SQL 정의**함. 
2) 3가지 준비하기  
	- 데이터베이스에 **커넥션** 준비. (Connetion)  
	- **PreparedStatement** -> (1) DB에 전달할 SQL과 (2)파라미터로 전달할 데이터들을 준비함. 서버에서 DB로 넣는 input  
	- **ResultSet** -> select 쿼리의 결과가 순서대로 들어간다 .  DB에서 서버로 나오는 output.
	- 예시) select member_id, money 라고 지정하면 member_id , money 라는 이름으로 데이터가 저장함.  
3) 커넥션을 통해 **SQL 전달**
4) **rs. next** -> if/while을 통해 커서 이동.
5) 빈칸이면, 새로운 **회원 객체만들고**, 상태값 넣기 후 리턴  
6) **리소스 정리 역순**으로 하기


#### 📌ExecuteQuery, ExecuteUpdate 차이점
 - ExecuteQuery -> 수행결과로 **ResultSet 객체의 값**을 반환  
 - ExecuteUpdate -> 수행결과로 **Int 타입의 값**을 반환  
출처 : [[JAVA] Execute, ExecuteQuery, ExecuteUpdate 차이점 알아보기 ](https://mozi.tistory.com/26)


#### 📌rs .next는 왜 있지?  
- rs는 현재 **커서의 위치**를 나타낸다고 생각하면 됌.  
- rs. next를 통해서 커서가 이동함(if는 단일, while 복수 일때)
- 데이터베이스 찾는 흐름  
![[Pasted image 20221130163139.png]]


#### 📌<실습> 수정/삭제 로직
- 수정 로직
```java
public void update(String memberId, int money) throws SQLException {  
    String sql = "update member set money=? where member_id=?";  
  
    Connection con = null;  
    PreparedStatement pstmt = null;  
  
    try {  
        con = getConnection();  
        pstmt = con.prepareStatement(sql);  
        pstmt.setInt(1, money);  
        pstmt.setString(2, memberId);  
        int resultSize = pstmt.executeUpdate();  
        log.info("resultSize={}", resultSize);  
    } catch (SQLException e) {  
        log.error("db error", e);  
        throw e;  
    } finally {  
        close(con, pstmt, null);  
    }  
  
}
```

- 삭제 로직
```java
public void delete(String memberId) throws SQLException {  
    String sql = "delete from member where member_id=?";  
  
    Connection con = null;  
    PreparedStatement pstmt = null;  
  
    try {  
        con = getConnection();  
        pstmt = con.prepareStatement(sql);  
        pstmt.setString(1, memberId);  
        pstmt.executeUpdate();  
    } catch (SQLException e) {  
        log.error("db error", e);  
        throw e;  
    } finally {  
        close(con, pstmt, null);  
    }  
  
}
```

# 커넥션풀과 데이터소스
## 커넥션 풀 이해
#### 📌커넥션 연결 매커니즘  
![[Pasted image 20221202231835.png]]
- 1. 애플리케이션 로직은 DB 드라이버를 통해 **커넥션을 조회**한다.  
- 2. DB 드라이버는 **DB와 TCP/IP 커넥션을 연결**한다. 물론 이 과정에서 3 way handshake 같은 TCP/IP 연결을 위한 네트워크 동작이 발생한다.  	
- 3. DB 드라이버는 TCP/IP 커넥션이 연결되면 ID, PW와 기타 **부가정보를 DB에 전달**한다.  
- 4. DB는 ID, PW를 통해 내부 인증을 완료하고, **내부에 DB 세션을 생성**한다.  
- 5. DB는 커넥션 **생성 완료**되었다는 응답을 보낸다.  
- 6. DB 드라이버는 클라이언트에 **커넥션 객체를 생성해서 반환**한다.


#### 📌커넥션 사용하기전에 문제점
- 새로 만드는 과정 복잡함.
- DB드라이버가  **커넥션을 매번 생성 획득** 으로 인한 시간 낭비 
- 해결법 : 커넥션을 커넥션풀에 미리 생성해두고 사용하자


#### 📌커낵션풀 사용
- 초기화
커넥션 풀은 필요한 만큼 커넥션을 미리 확보해서 풀에 보관함.
는 커넥션은 TCP/IP로 DB와 커넥션이 연결되어 있는 상태이기 때문에 언제든지 즉시 SQL을 DB에 전달가능함.
![[Pasted image 20221203173859.png]]
![[Pasted image 20221203173909.png]]

- 커넥션 사용
![[Pasted image 20221202232023.png]]
![[Pasted image 20221202232029.png]]


#### 📌커낵션풀 사용 매커니즘  
- 1. 커넥션 풀을 통해 **이미 생성되어 있는 커넥션을 객체 참조 조회**하기  
- 2. 커넥션 풀은 자신이 가지고 있는 **커넥션 중에 하나를 반환**한다.  
- 3. 애플리케이션 로직은 커넥션 풀에서 받은 커넥션을 사용해서 SQL을 데이터베이스에 전달하고 그 결과를 받아서 **처리**하기  
- 4. 다음에 다시 사용할 수 있도록 해당 커넥션을 그대로 **커넥션 풀에 반환**하면 된다. (종료 X)


#### 📌커넥션 풀 사용 장점  
- DB에 무한정 연결이 생성되는 것을 막아주어서 **DB를 보호**하는 효과가 있음  
- 커넥션을 새로 만드는 시간이 없기 때문에 **응답 속도**를 빠르게함.


## DataSource
#### 📌 DataSource 이해
- DataSource란? **커넥션 풀을 추상화**한것.
- DriverManager 를 통해서 커넥션을 획득하다가, 다른 커넥션 풀을 사용하는 방법으로 **변경할때 문제**생김.
- 해결책 : DataSource **인터페이스에만 의존**하도록 애플리케이션 로직을 작성 (추상화)
![[Pasted image 20221202234958.png]]


#### 📌 <실습> 기존의 DriverManager를 통해 커넥션을 획득해 사용하는 코드
```java
@Test
 void driverManager() throws SQLException {
	 Connection con1 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
	 Connection con2 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
	 log.info("connection={}, class={}", con1, con1.getClass());
	 log.info("connection={}, class={}", con2, con2.getClass());
 }
```

- 결과
```
connection=conn0: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnectionconnection=conn1: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnection
```


#### 📌 <실습> 스프링이 제공하는 DriverManagerDataSource를 통해 커넥션을 획득해 사용하는 코드
```java
@Test  
void dataSourceDriverManager() throws SQLException {  
    //DriverManagerDataSource - 항상 새로운 커넥션 획득  
    DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD); 
    useDataSource(dataSource);  
}  


private void useDataSource(DataSource dataSource) throws SQLException {  
    Connection con1 = dataSource.getConnection();  
    Connection con2 = dataSource.getConnection();  
    log.info("connection={}, class={}", con1, con1.getClass());  
    log.info("connection={}, class={}", con2, con2.getClass());  
}
```

- 결과
```
DriverManagerDataSource - Creating new JDBC DriverManager Connection to 
[jdbc:h2:tcp:..test]
DriverManagerDataSource - Creating new JDBC DriverManager Connection to 
[jdbc:h2:tcp:..test]
connection=conn0: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnection
connection=conn1: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnection
```


#### 📌 DriverManager와 DriverManagerDataSource 둘은 무슨 차이가 있지?  
- **설정과 사용의 분리**했음.  
	- DriverManager 는 커넥션을 획득할 때 마다 URL , USERNAME , PASSWORD 같은 **파라미터를 계속 전달함.**  
	- DataSource 를 사용하는 방식은 처음 객체를 생성할 때만 필요한 파리미터를 넘겨두고, 커넥션을 획득할 때는 단순히 dataSource.getConnection() 만 호출함.


#### 📌 <실습> 커넥션 풀을 초기 설정하는 코드
```java
@Test  
void dataSourceConnectionPool() throws SQLException, InterruptedException {  
    //커넥션 풀링: HikariProxyConnection(Proxy) -> JdbcConnection(Target)  
    HikariDataSource dataSource = new HikariDataSource();  
    dataSource.setJdbcUrl(URL);  
    dataSource.setUsername(USERNAME);  
    dataSource.setPassword(PASSWORD);  
    dataSource.setMaximumPoolSize(10);  
    dataSource.setPoolName("MyPool");  
    useDataSource(dataSource);  
    Thread.sleep(1000); //커넥션 풀에서 커넥션 생성 시간 대기  
}
```

- 출력
```
#커넥션 풀 초기화 정보 출력
HikariConfig - MyPool - configuration:
HikariConfig - maximumPoolSize................................10
HikariConfig - poolName................................"MyPool"

#커넥션 풀 전용 쓰레드가 커넥션 풀에 커넥션을 10개 채움
[MyPool connection adder] MyPool - Added connection conn0: url=jdbc:h2:.. 
user=SA
...
[MyPool connection adder] MyPool - Added connection conn9: url=jdbc:h2:.. 
user=SA

#커넥션 풀에서 커넥션 획득1
ConnectionTest - connection=HikariProxyConnection@446445803 wrapping conn0: 
url=jdbc:h2:tcp://localhost/~/test user=SA, class=class 
com.zaxxer.hikari.pool.HikariProxyConnection

#커넥션 풀에서 커넥션 획득2
ConnectionTest - connection=HikariProxyConnection@832292933 wrapping conn1: 
url=jdbc:h2:tcp://localhost/~/test user=SA, class=class 
com.zaxxer.hikari.pool.HikariProxyConnection
MyPool - After adding stats (total=10, active=2, idle=8, waiting=0)
```

- 매커니즘  
	- 1) 커넥션 풀 **구현체 생성**  
	- 2) **기본 설**정(URL, NAME, PASSWORD, PoolSize)   
	- 3) **커넥션 풀에서 사용**하기

- 커넥션 풀에서 커넥션을 생성하는 작업은 애플리케이션 실행 속도에 영향을 주지 않기 위해 별도의 쓰레드에서 작동함.  


#### 📌 <실습> 비즈니스 로직에 DataSource 적용하는 코드
```java
/**  
 * JDBC - DataSource 사용, JdbcUtils 사용  
 */  
@Slf4j  
public class MemberRepositoryV1 {  
    private final DataSource dataSource;  
    public MemberRepositoryV1(DataSource dataSource) 
    { this.dataSource = dataSource; }  
    
    //save()...  
    //findById()...    
    //update()....    
    //delete()....    
    
    private void close(Connection con, Statement stmt, ResultSet rs) {  
        JdbcUtils.closeResultSet(rs);  
        JdbcUtils.closeStatement(stmt);  
        JdbcUtils.closeConnection(con);  
    }  

    private Connection getConnection() throws SQLException {  
        Connection con = dataSource.getConnection();  
        log.info("get connection={}, class={}", con, con.getClass());  
        return con;  
    }
```

- J**dbcUtils 을 사용**하면 커넥션을 좀 더 편리하게 닫을 수 있다.

- DriverManagerDataSource -> HikariDataSource 로 변경해도 MemberRepositoryV1 의 코드는 전혀 변경하지 않아도 된다. 
 MemberRepositoryV1 는 DataSource **인터페이스에만 의존**하기 때문이다.


# 트랜잭션과 DB락
## 트랜잭션 이해
#### 📌 트랜잭션을 사용하는 이유는 뭘까?
- 하나의 논리적인 작업의 단위이기 때문에, 여러개의 작업을 하나의 논리적인 단위로 묶어서 반영과 복구를 조정할 수 있기 위해 사용
- 예시) 계좌이체 도중 오류가 났다면? 트랜잭션을 사용하지 않은 경우 출금은 됐지만 입금이 안되는 경우 발생할 수 있음.

#### 📌 트랜잭션 ACID
- 원자성  
    - 트랜잭션 내에서 실행한 작업들은 마치 **하나의 작업인 것처럼** 모두 성공 하거나 모두 실패해야한다.  
- 일관성  
    - 모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다. 예를 들어 데이터베이스에서 정한 **무결성 제약 조건**을 항상 만족해야 한다.  

- 격리성  
    - 동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지 않도록 격리한다. 예를 들어 동시에 같은 데이터를 수정하지 못하도록 해야 한다. 격리성은 동시성과 관련된 성능 이슈로 인해 트랜잭션 격리 수준 (Isolation level)을 선택할 수 있다.  
- 지속성  
    - 트랜잭션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템에 문제가 발생해도 데이터베이스 로그 등을 사용해서 성공한 트랜잭션 내용을 복구해야 한다.


#### 📌  무결성 제약조건이란? 
1) 개체 무결성 : 기본키는 null 값이 될수 없음
2) 참조 무결성 : 외래키는 참조할 수 없는 값을 가질수 없음
3) 도메인 무결성 : 특정 속성값은 그 속성이 정의돈 도메인에 속한 값이어야함(enum과 같은 개념)
4) 키 무결성 : 릴레이션에는 최소한 하나의 키가 존재해야함.
5) null 무결성 : 특정 속성은 null값을 가질 수  없음
6) 고유 무결성 : 특정 속성값은 서로 달라야함.
출처 : [[DB 데이터베이스] 무결성 제약조건 ](https://iingang.github.io/posts/DB-Integrity-constraint/)


#### 📌 데이터베이스 연결 구조와 DB 세션 설명
![[Pasted image 20221203222930.png]]
- 웹 애플리케이션 서버(WAS)나 DB 접근 툴 같은 클라이언트를 사용해서 데이터베이스 서버에 접근함. 클라언트와 서버는 역할의 개념이다!!  
- 데이터 베이스 서버은 커넥션을 통한 **모든 요청은 해당 세션**을 통해서 실행함.  
- 세션 종료하는 방법  
    - 트랜잭션을 시작하고, 커밋 또는 롤백을 통해 **트랜잭션을 종료**한다  
    - 사용자가 커넥션을 닫거나, 또는 DBA(DB 관리자)가 **세션을 강제로 종료**하면 세션은 종료된다.
-  세션이란? 데이터베이스 접속을 시작으로, 여러 작업을 수행한 후 접속 종료까지의 전체 기간을 의미한다.
출처 : [[Oracle SQL] 트랜잭션, 세션, 데이터 정의어(DDL)](https://developer-alle.tistory.com/203)


#### 📌<실습> 트랜잭션 계좌이체 예시
1) 두개의 다른 세션이 같은 테이블 값을 가지고 있게 만들기(기본설정)
![[Pasted image 20221203225610.png]]
```SQL
//데이터 초기화
set autocommit true;
delete from member;
insert into member(member_id, money) values ('oldId',10000);
```


2) 신규데이터 추가하면 해당 세션만 데이터 확인 가능
![[Pasted image 20221203225659.png]]
```SQL
//트랜잭션 시작
set autocommit false; //수동 커밋 모드
insert into member(member_id, money) values ('newId1',10000);
insert into member(member_id, money) values ('newId2',10000);
```


3) if 커밋하면? 다른 세션에서도 회원 테이블을 조회하면 신규 회원들을 확인할 수 있음.
![[Pasted image 20221203225738.png]]
```SQL
commit; //데이터베이스에 반영
```

4) if 롤백하면? 모두 트랜잭션을 시작하기 직전의 상태로 복구됨.
![[Pasted image 20221203225812.png]]
```SQL
rollback; //롤백으로 데이터베이스에 변경 사항을 반영하지 않는다.
```

5) if 오류가 발생하면
![[Pasted image 20221203230237.png]]
``` sql
set autocommit false;
update member set money=10000 - 2000 where member_id = 'memberA'; 
//쿼리 예외발생
update member set money=10000 + 2000 where member_iddd = 'memberB'; 
```
- memberA 의 돈은 2000원 줄어들었지만, memberB 의 돈은 2000원 증가하지 않았다는 점이다. 결과적으로 계좌이체는 실패하고 memberA 의 돈만 2000원 줄어든 상황.  
- 이때 강제로 커밋하면, 문제상황 유지  
- 롤백하면 원래 데이터로 복구


#### 📌 커밋하지 않은 데이터를 다른 곳에서 조회할 수 있으면 어떤 문제가 발생할까?
- 데이터 정합성에 큰 문제가 발생  
- 예) A세션이 임시 상태에 있는 데이터 만들었다. 해당 데이터를 B세션이 데이터를 변경하고 저장했지만, A세션이 롤백하면 데이터 사라짐.


#### 📌 정합성과 무결성의 차이는?
- 정합성이란? 어떤 데이터들이 값이 서로 일치하는 상태를 의미
- 무결성이란? 데이터 값이 정확한 상태
- 정합성 O, 무결성 X 예시
	주문정보 테이블에서 고객번호가 모두 -1으로 입력되어 있고,  고객정보 테이블에도 -1의 값을 갖는 고객이 존재한다. (데이터의 값이 일치한다.) 그러나 고객번호는 반드시 1 이상의 값을 가져야 한다. (데이터의 값이 정확하지 않다.) 이런 상황에는 데이터 정합성은 이상이 없으나, 데이터 무결성은 훼손되었다고 볼 수 있다.
- 정합성 X, 무결성 O 예시
	위 예시에서 주문정보 테이블의 고객번호를 -1에서 2로 변경했지만, 고객정보 테이블에는 고객번호가 변경되지 않았을 때, (데이터의 값이 서로 일치하지 않는다.)  데이터 정합성이 훼손되었다고 볼 수 있다.
출처 : [무결성과 정합성이란 무엇인가? (velog.io)](https://velog.io/@yangsijun528/%EB%AC%B4%EA%B2%B0%EC%84%B1%EA%B3%BC-%EC%A0%95%ED%95%A9%EC%84%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)


#### 📌 자동커밋 VS 수동 커밋
- 수동 커밋 모드나 자동 커밋 모드는 한번 설정하면 해당 세션에서는 계속 유지된다. 중간에 변경하는 것은 가능

- 자동 커밋 설정
``` SQL
set autocommit true; //자동 커밋 모드 설정
insert into member(member_id, money) values ('data1',10000);
insert into member(member_id, money) values ('data2',10000);
```
장점 : 커밋이나 롤백을 직접 호출하지 않아도 되는 편리함.
단점 : 원하는 트랜잭션 기능을 제대로 사용할 수 없음.

- 수동 커밋 설정
``` SQL
set autocommit false;
insert into member(member_id, money) values ('data1',10000);
insert into member(member_id, money) values ('data2',10000);
commit; //수동 커밋
```
장점 : 트랙젹션 기능 사용.
단점 : 커밋/롤백을 일일히 쳐줘야함.

## DB 락
#### 📌 DB 락 개념 설명
- DB락이란? 세션이 트랜잭션을 시작하고 데이터를 수정하는 동안에는 커밋이나 롤백 전까지 다른 세션에서 해당 데이터를 수정할 수 없게 막아야 한다. why? 트랜잭션의 원자성이 깨짐.
- 일반적인 조회에는 데이터가 변경되지 않기 때문에  DB락을 사용 X . 그러나 락을 획득하고 싶다면 `for update` 구문사용
```
select * from member where member_id='memberA' for update;
```
- 오랜시간 락을 획득하지 못하면 타임 아웃 오류 발생함.
![[Pasted image 20221204000916.png]]


#### 📌<실습> DB 락 변경 예시
1) 기본 데이터를 입력하자.
![[Pasted image 20221203231516.png]]
```sql
set autocommit true;  //자동 커밋 설정
delete from member;  //초기화
insert into member(member_id, money) values ('memberA',10000);
```

2) 세션1은 트랜잭션을 시작한다. 락이 남아 있으므로 세션1은 락을 획득한다.
![[Pasted image 20221203231528.png]]
```sql
set autocommit false;
update member set money=500 where member_id = 'memberA';
```

3) 세션1이 커밋을 한다. 커밋으로 트랜잭션이 종료되었으므로 락도 반납한다.
![[Pasted image 20221203231534.png]]
```sql
SET LOCK_TIMEOUT 60000;
set autocommit false;
update member set money=1000 where member_id = 'memberA';
```

4) 세션1이 커밋을 한다. 커밋으로 트랜잭션이 종료되었으므로 락도 반납한다.
![[Pasted image 20221203231657.png]]
```sql
commit;
```

5) 타임 아웃전, 세션2가 락을 확득한다. update sql을 수행한다.
![[Pasted image 20221203231705.png]]
![[Pasted image 20221203231709.png]]

6) 세션2는 커밋을 수행하고 트랜잭션이 종료되었으므로 락을 반납한다.
![[Pasted image 20221203231726.png]]
```sql
commit;
```

## 로직과 트랜잭션
#### 📌 <실습>트랜잭션 없이 Service 계층 구현
- 트랜잭션 없는 계좌이체 예시
```java
@RequiredArgsConstructor  
public class MemberServiceV1 {  
    private final MemberRepositoryV1 memberRepository;  
    
    public void accountTransfer(String fromId, String toId, int money) throws SQLException {  
    
        Member fromMember = memberRepository.findById(fromId);  
        Member toMember = memberRepository.findById(toId);  
        memberRepository.update(fromId, fromMember.getMoney() - money);  
        validation(toMember);  
        memberRepository.update(toId, toMember.getMoney() + money);  
    }  
    
    private void validation(Member toMember) {  
        if (toMember.getMemberId().equals("ex")) {  
            throw new IllegalStateException("이체중 예외 발생");  
        }  
    }  
}
```


#### 📌 Repository와 Service 계층의 차이?
- Service 계층
Controller와 Dao의 중간 영역에서 사용함.
Repository의 메소드를 여러 개 사용해 비즈니스 로직을 구성함.

- Repository 계층
Database 와 같이 데이터 저장소에 접근하는 영역임.
CRUD를 통해 데이터에 접근함.

출처 : [스프링 프로젝트 계층 구조 이해](https://blog.naver.com/ghdalswl77/222489613651)


#### 📌 트랜잭션을 어떤 계층에 걸어야 할까?
![[Pasted image 20221204001320.png]]
- 트랜잭션은 비즈니스 로직이 있는 **서비스 계층에서 시작**해야 한다. why? 비즈니스 로직으로 인해 문제가 되는 부분을 함께 롤백해야 하기 때문이다.  
- **트랜잭션을 시작하려면 커넥션이 필요**함. 그리고 **트랜잭션을 사용하는 동안 같은 커넥션을 유지**해야함. HOW? 파라미터로 커넥션을 전달하여 사용하자!!


#### 📌 <실습> 트랜잭션 추가한 코드
- Respositoy 계층 트랜잭션 추가
```java
...
public Member findById(Connection con, String memberId) throws SQLException {  
    String sql = "select * from member where member_id = ?";  
  
    PreparedStatement pstmt = null;  
    ResultSet rs = null;  
  
    try {  
	    //con = getConnection(); 없어짐
        pstmt = con.prepareStatement(sql);  
        pstmt.setString(1, memberId);  
  
        rs = pstmt.executeQuery();  
        if (rs.next()) {  
            Member member = new Member();  
            member.setMemberId(rs.getString("member_id"));  
            member.setMoney(rs.getInt("money"));  
            return member;  
        } else {  
            throw new NoSuchElementException("member not found memberId=" + memberId);  
        }  
  
    } catch (SQLException e) {  
        log.error("db error", e);  
        throw e;  
    } finally {  
        //connection은 여기서 닫지 않는다.  
        JdbcUtils.closeResultSet(rs);  
        JdbcUtils.closeStatement(pstmt);  
        //JdbcUtils.closeConnection(con); 없어짐
    }  
  
}
...
```
- 같은 커넥션을 유지하기 위해 새롭게 함수 정의해야함. 파라미터로 커넥션 넘겨주고 `con = getConnection()` 를 없앤다. why? 새로운 커넥션을 획득하지 않고 파라미터로 넘겨 받을거기 때문이다.  
- finally에서 커넥션 종료하지 말고, 서비스 로직에서 종료하기
- findById와 같이 다른 메소드(update)도 변경해주기


- Service 계층에 트랜잭션 추가
```java
@RequiredArgsConstructor  
public class MemberServiceV2 {  
    private final DataSource dataSource;  
    private final MemberRepositoryV2 memberRepository;  

    public void accountTransfer(String fromId, String toId, int money) throws SQLException {  
	    //이제 여기서 커넥션은 얻어야 해서 추가
        Connection con = dataSource.getConnection(); 
         
        try {  
            con.setAutoCommit(false);//트랜잭션 시작  
            //비즈니스 로직  
            bizLogic(con, fromId, toId, money);  
            con.commit(); //성공시 커밋  
            
        } catch (Exception e) {  
            con.rollback(); //실패시 롤백  
            throw new IllegalStateException(e); 
             
        } finally {  
            release(con);  
        }  
  
    }  

...

	/*리소스 정리*/
    private void release(Connection con) {  
        if (con != null) {  
            try {  
                con.setAutoCommit(true); //기본값으로 커넥션 풀 고려  
                con.close();  
            } catch (Exception e) {  
                log.info("error", e);  
            }  
        }  
    }
}
```
- Connection con = dataSource.getConnection() 으로 하나의 커넥션 유지하기
- con.setAutoCommit(false) 으로 트랜잭션 시작
- release(con) 으로 기본 값인 자동 커밋 모드로 변경하는 것 + 커넥션에 반납하기

#### 📌 


#### 📌 


#### 📌 


#### 📌 


#### 📌 


#### 📌 


#### 📌 