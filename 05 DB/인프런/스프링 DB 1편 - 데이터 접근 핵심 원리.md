# JDBC
## H2 ์ค์ 
#### ๐<์ค์ต> Intelij์ ์ฐ๊ฒฐํ๊ธฐ
- ์์กด์ฑ ์ถ๊ฐํ๊ธฐ(build.gradle)
```
dependencies {
	...
	runtimeOnly 'com.h2database:h2'
	...
}
```


#### ๐<์ค์ต> H2 ๋ฐ์ดํฐ๋ฒ ์ด์ค ์๋ฒ ์คํ
- ํ์ผ ์์ฑ ๋ฐฉ๋ฒ
jdbc:h2:~/test (์ต์ด ํ๋ฒ) ์ดํ๋ถํฐ๋ jdbc:h2:tcp://localhost/~/test ์ด๋ ๊ฒ ์ ์


#### ๐<์ค์ต> ํ์ด๋ธ ์์ฑํ๊ธฐ
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


## JDBC ์ดํด
#### ๐๋ฐ์ดํฐ ๋ฒ ์ด์ค๋ฅผ ์ฐ๋ ์ด์ ๋?  
- ์ ํ๋ฆฌ์ผ์ด์์ ๊ฐ๋ฐํ  ๋ ์ค์ํ **๋ฐ์ดํฐ๋ ๋๋ถ๋ถ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ณด๊ด**ํจ. ์๋ฐ์ฝ๋์ ๋ณด๊ดํ๋ฉด, ์๋ฐ๊ฐ ์ข๋ฃ๋๋ฉด ๋ฐ์ดํฐ๊ฐ ๋ ๋ผ๊ฐ.

#### ๐์๋ฒ์ DB ์ฌ์ฉ๋ฒ์?
- ๋์ ๊ณผ์  ๊ทธ๋ฆผ์ผ๋ก ์ดํด
![[Pasted image 20221130152811.png]]
- ๋งค์ปค๋์ฆ  
    - 1. **์ปค๋ฅ์ ์ฐ๊ฒฐ**: ์ฃผ๋ก TCP/IP๋ฅผ ์ฌ์ฉํด์ ์ปค๋ฅ์์ ์ฐ๊ฒฐํ๋ค.  
    - 2. **SQL ์ ๋ฌ**: ์ ํ๋ฆฌ์ผ์ด์ ์๋ฒ๋ DB๊ฐ ์ดํดํ  ์ ์๋ SQL์ ์ฐ๊ฒฐ๋ ์ปค๋ฅ์์ ํตํด DB์ ์ ๋ฌํ๋ค.  
    - 3. **๊ฒฐ๊ณผ ์๋ต**: DB๋ ์ ๋ฌ๋ SQL์ ์ํํ๊ณ  ๊ทธ ๊ฒฐ๊ณผ๋ฅผ ์๋ตํ๋ค. ์ ํ๋ฆฌ์ผ์ด์ ์๋ฒ๋ ์๋ต ๊ฒฐ๊ณผ๋ฅผ ํ์ฉํ๋ค.


#### ๐JDBC ์ญํ ์?
- ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์๋ฐ๋ฅผ **์ฐ๊ฒฐํ๋ ๋งค๊ฐ์ฒด**๊ฐ JDBC API.


#### ๐JDBC์ 2๊ฐ์ง ๋ฌธ์ ์ 
- 1) ๋ค๋ฅธ ์ข๋ฅ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ก **๋ณ๊ฒฝํ๋ฉด** ์ ํ๋ฆฌ์ผ์ด์ ์๋ฒ์ ๊ฐ๋ฐ๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฌ์ฉ์ฝ๋๋ ํจ๊ป ๋ณ๊ฒฝํด์ผ ํ๋ค.  
- 2) ๊ฐ๊ฐ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ง๋ค ์ปค๋ฅ์ ์ฐ๊ฒฐ, SQL ์ ๋ฌ, ๊ทธ๋ฆฌ๊ณ  ๊ทธ ๊ฒฐ๊ณผ๋ฅผ ์๋ต ๋ฐ๋ ๋ฐฉ๋ฒ์ ์๋ก ํ์ตํด์ผ ํ๋ค.  ์ฆ, ๋ฐ์ดํฐ ๋ฒ ์ด์ค๋ง๋ค **ํตํฉ X**


#### ๐JDBC ํ์ค ์ธํฐํ์ด์ค์ ํด๊ฒฐ๋ฐฉ๋ฒ์?  
- ์ฒซ๋ฒ์งธ ๋ฌธ์  ํด๊ฒฐ์ ์ํด, ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง์ ์ด์  **JDBC ํ์ค ์ธํฐํ์ด์ค**์๋ง ์์กดํ๋ค.  ๋ฐ๋ผ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ๋ค๋ฅธ ์ข๋ฅ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ก ๋ณ๊ฒฝํ๊ณ  ์ถ์ผ๋ฉด JDBC ๊ตฌํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ง ๋ณ๊ฒฝํ๋ฉด ๋๋ค.
- ๋๋ฒ์งธ ๋ฌธ์  ํด๊ฒฐ์ ์ํด, JDBC **ํ์ค ์ธํฐํ์ด์ค ์ฌ์ฉ๋ฒ**๋ง ํ์ตํ๋ฉด ๋๋ค.  ํ๋ฒ ๋ฐฐ์๋๋ฉด ์์ญ๊ฐ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ชจ๋ ๋์ผํ๊ฒ ์ ์ฉํ  ์ ์๋ค.


#### ๐JDBC ํ์ค ์ธํฐํ์ด์ค ๋์ ๋ฐฉ๋ฒ์?
- ์ธํฐํ์ด์ค ๊ตฌํ
![[Pasted image 20221130152951.png]]
- ๊ธฐ์กด ๋ฐฉ์๊ณผ ์ฐ๊ฒฐํ๋ ๋ฐฉ๋ฒ์ ๋์ผํจ.
![[Pasted image 20221130153011.png]]


## JDBC์ ์ต์  ๋ฐ์ดํฐ ์ ๊ทผ ๊ธฐ์ 
#### ๐์ต์  ๊ธฐ์ ์ ์ ์จ์ผํ ๊น?  
- ์์ฑํด์ผ๋  ์ฝ๋๊ฐ ๋๋ฌด ๋ง๊ณ  ๋ฐ๋ณต์ ์ด๋ผ์ ์ฌ์ฉํ๊ธฐ์ **๋ณต์กํจ**
- JDBC ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ๋ก์ง
    ![[Pasted image 20221130153045.png]]


#### ๐SQL Mapper
- ๋ก์ง์? ๊ฐ์ฒด์ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ฐ์ดํฐ๋ฅผ **๊ฐ๋ฐ์๊ฐ ์์ฑํ SQL๋ก ๋งคํ์์ผ์ฃผ๋ ํ๋ ์์ํฌ**
![[Pasted image 20221130153129.png]]
- ์ฅ์   
    - SQL ์๋ต ๊ฒฐ๊ณผ๋ฅผ **๊ฐ์ฒด๋ก ํธ๋ฆฌํ๊ฒ ๋ณํ**ํด์ค๋ค.  
    - JDBC์ **๋ฐ๋ณต ์ฝ๋๋ฅผ ์ ๊ฑฐ**ํด์ค๋ค.  
-   ๋จ์   
    - ๊ฐ๋ฐ์๊ฐ **SQL์ ์ง์  ์์ฑ**ํด์ผํ๋ค.


#### ๐ORM ๊ธฐ์   
- ๋ก์ง์? ๊ฐ์ฒด์ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ฐ์ดํฐ๋ฅผ **์๋์ผ๋ก ๋งคํ**์์ผ์ฃผ๋ ํ๋ ์์ํฌ
![[Pasted image 20221130153226.png]]
- ์ฅ์   
    - ์ฌ์ฌ์ฉ **์ ์ง ๋ณด์ ํธ๋ฆฌ์ฑ์ด ์ฆ๊ฐ**ํ๋ค.  
    - ๊ฐ์ฒด ์งํฅ์ ์ธ ์ฝ๋๋ก **์ง๊ด์ ์ธ ๋น์ง๋์ค ๋ก์ง**์ ์์ฑํ  ์ ์๋ค.  
- ๋จ์   
	- ์ด๊ธฐ ์ ์ฉ์ ๋ฌ๋ ์ปค๋ธ๊ฐ ์ด๋ ์ ๋ ์๋ค.  
	- **๋ณต์กํ SQL**์ ์ฌ์ฉํ์ง ๋ชปํ  ์ ๋ ์๋ค.


#### ๐<์ค์ต> H2 ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐ๊ฒฐ
1) ์ ์ ๊ธฐ๋ณธ์ ๋ณด ๊ธฐ์
์ ์ํ๋๋ฐ ํ์ํ ๊ธฐ๋ณธ ์ ๋ณด๋ฅผ ํธ๋ฆฌํ๊ฒ ์ฌ์ฉํ  ์ ์๋๋ก ์์๋ง๋ค๊ธฐ.
```java
public abstract class ConnectionConst {  
    public static final String URL = "jdbc:h2:tcp://localhost/~/test";  
    public static final String USERNAME = "sa";  
    public static final String PASSWORD = "";  
}
```


2) ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ฐ๊ฒฐ ์ฝ๋
JDBC๋ฅผ ์ฌ์ฉํด์ ์ค์  ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ฐ๊ฒฐํ๋ ์ฝ๋ ๋ง๋ค๊ธฐ. 
H2 ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋๋ผ์ด๋ฒ๊ฐ ์๋ํด์ ์ค์  ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ปค๋ฅ์์ ๋งบ๊ณ  ๊ทธ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํด์ค๋ค. (์๋์ผ๋ก ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐพ์)
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


#### ๐JDBC DriverManager ์ฐ๊ฒฐ ์ดํด
![[Pasted image 20221130160330.png]]
- ๋งค์ปค๋์ฆ
	- 1. ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง์์ **์ปค๋ฅ์์ ์์ฒญ**ํ๋ฉด DriverManager. getConnection() ์ ํธ์ถ  why? ํด๋น ๋ฐ์ดํฐ ๋ฒ ์ด์ค ์ฐ๊ฒฐํ๊ธฐ ์ํจ
	- 2. DriverManager ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ ๋ฑ๋ก๋ ๋๋ผ์ด๋ฒ **๋ชฉ๋ก์ ์๋์ผ๋ก ์ธ์**ํ๋ค. ์ด ๋๋ผ์ด๋ฒ๋ค์๊ฒ ์์๋๋ก ๋ค์ ์ ๋ณด(URL, ID, PASSWORD)๋ฅผ ๋๊ฒจ์ **์ปค๋ฅ์์ ํ๋ํ  ์ ์๋์ง ํ์ธ**.  
	- 3. ๊ตฌํ์ฒด๊ฐ ํด๋ผ์ด์ธํธ์ **์ฐพ์ ์ปค๋ฅ์ ๋ฐํ**.


## JDBC ๊ฐ๋ฐ
#### ๐<์ค์ต> ๋ก์ง ๋ง๋ค๊ธฐ์ ์ ์์ฑํ  ๊ฒ๋ค
- SQL
ํ์ด๋ธ๊ณผ ์ํ ๋ฐ์ดํฐ ๋ง๋ค๊ธฐ๋ฅผ ํตํด์ member ํ์ด๋ธ์ ๋ฏธ๋ฆฌ ๋ง๋ค์ด๋์ด์ผ ํ๋ค.
```sql
drop table member if exists cascade;
create table member (
	 member_id varchar(10),
	 money integer not null default 0,
	 primary key (member_id)
);
```

- Member : ํ์ ๊ฐ์ฒด ๋ง๋ค๊ธฐ
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


#### ๐<์ค์ต> ๋ฑ๋ก ๋ก์ง 
- MemberRepositoryV0 : ํ์ ๊ฐ์ฒด๋ฅผ ๋ฐ์ดํฐ ๋ฒ ์ด์ค์ ๋ฑ๋ก. (์ดํด๋ง)
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

#### ๐๋ฑ๋ก ๋งค์ปค๋์ฆ ์ค๋ช  
1) ์ ๋ฌํ  **SQL ์ ์**ํจ  
2) 2๊ฐ์ง ์ค๋นํ๊ธฐ  
	- ๋ฐ์ดํฐ๋ฒ ์ด์ค์ **์ปค๋ฅ์ ์ค๋น**. (Connetion)๊ณผ ์ ๋ฌํ  SQL  
	- **PreparedStatement** -> (1) DB์ ์ ๋ฌํ  SQL๊ณผ (2)ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ  ๋ฐ์ดํฐ๋ค์ ์ค๋นํจ.  
3) ์ปค๋ฅ์ ์ฐ๊ฒฐ์ con, ํ๋ผ๋ฏธํฐ ์ ๋ฌ์ pstmt๋ฅผ ํตํด **๋ฐ์ธ๋ฉ**ํจ.  
	- ๋ฐ์ธ๋ฉ์ด๋ ์ค๋ธ์ ํธ์ ํ๋กํผํฐ(๋ฉ์๋ set)์ ๊ฐ์ ๋ฃ๋ ๊ฒ์ ๋งํ๋ค.  
4) SQL์ ์ปค๋ฅ์์ ํตํด ์ค์  ๋ฐ์ดํฐ๋ฒ ์ด์ค์ **์ ๋ฌ**ํ๋ค.  
5) **๋ฆฌ์์ค ์ ๋ฆฌ ์ญ์**์ผ๋ก ํ๊ธฐ


#### ๐<์ค์ต> ์กฐํ ๋ก์ง
- MemberRepositoryV0 : ํ์ ๊ฐ์ฒด๋ฅผ ๋ฐ์ดํฐ ๋ฒ ์ด์ค์ ์กฐํ. (์ดํด๋ง)
```java
public Member findById(String memberId) throws SQLException {  
    String sql = "select * from member where member_id = ?";  
  
    Connection con = null;  
    PreparedStatement pstmt = null;  
    ResultSet rs = null;  
  
    try {  
        con = getConnection();  
        pstmt = con.prepareStatement(sql);  
        pstmt.setString(1, memberId);   //์ฐพ์ memberId๋ฅผ pstmt์ ์ ์ฅ  

		//์กฐํํ ๋ ์ฌ์ฉํ๋ ๋ช๋ น๋ฌธ์ผ๋ก ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํจ.  
        rs = pstmt.executeQuery();  
        if (rs.next()) {        //next๊ฐ ์์์ ๋ฃจํ๋ฅผ ๋๋ ค์ฃผ๋ ๊ตฐ. +  
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


#### ๐์กฐํ ๋งค์ปค๋์ฆ ์ค๋ช  
1) ์ ๋ฌํ  **SQL ์ ์**ํจ. 
2) 3๊ฐ์ง ์ค๋นํ๊ธฐ  
	- ๋ฐ์ดํฐ๋ฒ ์ด์ค์ **์ปค๋ฅ์** ์ค๋น. (Connetion)  
	- **PreparedStatement** -> (1) DB์ ์ ๋ฌํ  SQL๊ณผ (2)ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ  ๋ฐ์ดํฐ๋ค์ ์ค๋นํจ. ์๋ฒ์์ DB๋ก ๋ฃ๋ input  
	- **ResultSet** -> select ์ฟผ๋ฆฌ์ ๊ฒฐ๊ณผ๊ฐ ์์๋๋ก ๋ค์ด๊ฐ๋ค .  DB์์ ์๋ฒ๋ก ๋์ค๋ output.
	- ์์) select member_id, money ๋ผ๊ณ  ์ง์ ํ๋ฉด member_id , money ๋ผ๋ ์ด๋ฆ์ผ๋ก ๋ฐ์ดํฐ๊ฐ ์ ์ฅํจ.  
3) ์ปค๋ฅ์์ ํตํด **SQL ์ ๋ฌ**
4) **rs. next** -> if/while์ ํตํด ์ปค์ ์ด๋.
5) ๋น์นธ์ด๋ฉด, ์๋ก์ด **ํ์ ๊ฐ์ฒด๋ง๋ค๊ณ **, ์ํ๊ฐ ๋ฃ๊ธฐ ํ ๋ฆฌํด  
6) **๋ฆฌ์์ค ์ ๋ฆฌ ์ญ์**์ผ๋ก ํ๊ธฐ


#### ๐ExecuteQuery, ExecuteUpdate ์ฐจ์ด์ 
 - ExecuteQuery -> ์ํ๊ฒฐ๊ณผ๋ก **ResultSet ๊ฐ์ฒด์ ๊ฐ**์ ๋ฐํ  
 - ExecuteUpdate -> ์ํ๊ฒฐ๊ณผ๋ก **Int ํ์์ ๊ฐ**์ ๋ฐํ  
์ถ์ฒ : [[JAVA] Execute, ExecuteQuery, ExecuteUpdate ์ฐจ์ด์  ์์๋ณด๊ธฐ ](https://mozi.tistory.com/26)


#### ๐rs .next๋ ์ ์์ง?  
- rs๋ ํ์ฌ **์ปค์์ ์์น**๋ฅผ ๋ํ๋ธ๋ค๊ณ  ์๊ฐํ๋ฉด ๋.  
- rs. next๋ฅผ ํตํด์ ์ปค์๊ฐ ์ด๋ํจ(if๋ ๋จ์ผ, while ๋ณต์ ์ผ๋)
- ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐพ๋ ํ๋ฆ  
![[Pasted image 20221130163139.png]]


#### ๐<์ค์ต> ์์ /์ญ์  ๋ก์ง
- ์์  ๋ก์ง
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

- ์ญ์  ๋ก์ง
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

# ์ปค๋ฅ์ํ๊ณผ ๋ฐ์ดํฐ์์ค
## ์ปค๋ฅ์ ํ ์ดํด
#### ๐์ปค๋ฅ์ ์ฐ๊ฒฐ ๋งค์ปค๋์ฆ  
![[Pasted image 20221202231835.png]]
- 1. ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง์ DB ๋๋ผ์ด๋ฒ๋ฅผ ํตํด **์ปค๋ฅ์์ ์กฐํ**ํ๋ค.  
- 2. DB ๋๋ผ์ด๋ฒ๋ **DB์ TCP/IP ์ปค๋ฅ์์ ์ฐ๊ฒฐ**ํ๋ค. ๋ฌผ๋ก  ์ด ๊ณผ์ ์์ 3 way handshake ๊ฐ์ TCP/IP ์ฐ๊ฒฐ์ ์ํ ๋คํธ์ํฌ ๋์์ด ๋ฐ์ํ๋ค.  	
- 3. DB ๋๋ผ์ด๋ฒ๋ TCP/IP ์ปค๋ฅ์์ด ์ฐ๊ฒฐ๋๋ฉด ID, PW์ ๊ธฐํ **๋ถ๊ฐ์ ๋ณด๋ฅผ DB์ ์ ๋ฌ**ํ๋ค.  
- 4. DB๋ ID, PW๋ฅผ ํตํด ๋ด๋ถ ์ธ์ฆ์ ์๋ฃํ๊ณ , **๋ด๋ถ์ DB ์ธ์์ ์์ฑ**ํ๋ค.  
- 5. DB๋ ์ปค๋ฅ์ **์์ฑ ์๋ฃ**๋์๋ค๋ ์๋ต์ ๋ณด๋ธ๋ค.  
- 6. DB ๋๋ผ์ด๋ฒ๋ ํด๋ผ์ด์ธํธ์ **์ปค๋ฅ์ ๊ฐ์ฒด๋ฅผ ์์ฑํด์ ๋ฐํ**ํ๋ค.


#### ๐์ปค๋ฅ์ ์ฌ์ฉํ๊ธฐ์ ์ ๋ฌธ์ ์ 
- ์๋ก ๋ง๋๋ ๊ณผ์  ๋ณต์กํจ.
- DB๋๋ผ์ด๋ฒ๊ฐ  **์ปค๋ฅ์์ ๋งค๋ฒ ์์ฑ ํ๋** ์ผ๋ก ์ธํ ์๊ฐ ๋ญ๋น 
- ํด๊ฒฐ๋ฒ : ์ปค๋ฅ์์ ์ปค๋ฅ์ํ์ ๋ฏธ๋ฆฌ ์์ฑํด๋๊ณ  ์ฌ์ฉํ์


#### ๐์ปค๋ต์ํ ์ฌ์ฉ
- ์ด๊ธฐํ
์ปค๋ฅ์ ํ์ ํ์ํ ๋งํผ ์ปค๋ฅ์์ ๋ฏธ๋ฆฌ ํ๋ณดํด์ ํ์ ๋ณด๊ดํจ.
๋ ์ปค๋ฅ์์ TCP/IP๋ก DB์ ์ปค๋ฅ์์ด ์ฐ๊ฒฐ๋์ด ์๋ ์ํ์ด๊ธฐ ๋๋ฌธ์ ์ธ์ ๋ ์ง ์ฆ์ SQL์ DB์ ์ ๋ฌ๊ฐ๋ฅํจ.
![[Pasted image 20221203173859.png]]
![[Pasted image 20221203173909.png]]

- ์ปค๋ฅ์ ์ฌ์ฉ
![[Pasted image 20221202232023.png]]
![[Pasted image 20221202232029.png]]


#### ๐์ปค๋ต์ํ ์ฌ์ฉ ๋งค์ปค๋์ฆ  
- 1. ์ปค๋ฅ์ ํ์ ํตํด **์ด๋ฏธ ์์ฑ๋์ด ์๋ ์ปค๋ฅ์์ ๊ฐ์ฒด ์ฐธ์กฐ ์กฐํ**ํ๊ธฐ  
- 2. ์ปค๋ฅ์ ํ์ ์์ ์ด ๊ฐ์ง๊ณ  ์๋ **์ปค๋ฅ์ ์ค์ ํ๋๋ฅผ ๋ฐํ**ํ๋ค.  
- 3. ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง์ ์ปค๋ฅ์ ํ์์ ๋ฐ์ ์ปค๋ฅ์์ ์ฌ์ฉํด์ SQL์ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ๋ฌํ๊ณ  ๊ทธ ๊ฒฐ๊ณผ๋ฅผ ๋ฐ์์ **์ฒ๋ฆฌ**ํ๊ธฐ  
- 4. ๋ค์์ ๋ค์ ์ฌ์ฉํ  ์ ์๋๋ก ํด๋น ์ปค๋ฅ์์ ๊ทธ๋๋ก **์ปค๋ฅ์ ํ์ ๋ฐํ**ํ๋ฉด ๋๋ค. (์ข๋ฃ X)


#### ๐์ปค๋ฅ์ ํ ์ฌ์ฉ ์ฅ์   
- DB์ ๋ฌดํ์  ์ฐ๊ฒฐ์ด ์์ฑ๋๋ ๊ฒ์ ๋ง์์ฃผ์ด์ **DB๋ฅผ ๋ณดํธ**ํ๋ ํจ๊ณผ๊ฐ ์์  
- ์ปค๋ฅ์์ ์๋ก ๋ง๋๋ ์๊ฐ์ด ์๊ธฐ ๋๋ฌธ์ **์๋ต ์๋**๋ฅผ ๋น ๋ฅด๊ฒํจ.


## DataSource
#### ๐ DataSource ์ดํด
- DataSource๋? **์ปค๋ฅ์ ํ์ ์ถ์ํ**ํ๊ฒ.
- DriverManager ๋ฅผ ํตํด์ ์ปค๋ฅ์์ ํ๋ํ๋ค๊ฐ, ๋ค๋ฅธ ์ปค๋ฅ์ ํ์ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ์ผ๋ก **๋ณ๊ฒฝํ ๋ ๋ฌธ์ **์๊น.
- ํด๊ฒฐ์ฑ : DataSource **์ธํฐํ์ด์ค์๋ง ์์กด**ํ๋๋ก ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง์ ์์ฑ (์ถ์ํ)
![[Pasted image 20221202234958.png]]


#### ๐ <์ค์ต> ๊ธฐ์กด์ DriverManager๋ฅผ ํตํด ์ปค๋ฅ์์ ํ๋ํด ์ฌ์ฉํ๋ ์ฝ๋
```java
@Test
 void driverManager() throws SQLException {
	 Connection con1 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
	 Connection con2 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
	 log.info("connection={}, class={}", con1, con1.getClass());
	 log.info("connection={}, class={}", con2, con2.getClass());
 }
```

- ๊ฒฐ๊ณผ
```
connection=conn0: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnectionconnection=conn1: url=jdbc:h2:tcp://..test user=SA, class=class 
org.h2.jdbc.JdbcConnection
```


#### ๐ <์ค์ต> ์คํ๋ง์ด ์ ๊ณตํ๋ DriverManagerDataSource๋ฅผ ํตํด ์ปค๋ฅ์์ ํ๋ํด ์ฌ์ฉํ๋ ์ฝ๋
```java
@Test  
void dataSourceDriverManager() throws SQLException {  
    //DriverManagerDataSource - ํญ์ ์๋ก์ด ์ปค๋ฅ์ ํ๋  
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

- ๊ฒฐ๊ณผ
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


#### ๐ DriverManager์ DriverManagerDataSource ๋์ ๋ฌด์จ ์ฐจ์ด๊ฐ ์์ง?  
- **์ค์ ๊ณผ ์ฌ์ฉ์ ๋ถ๋ฆฌ**ํ์.  
	- DriverManager ๋ ์ปค๋ฅ์์ ํ๋ํ  ๋ ๋ง๋ค URL , USERNAME , PASSWORD ๊ฐ์ **ํ๋ผ๋ฏธํฐ๋ฅผ ๊ณ์ ์ ๋ฌํจ.**  
	- DataSource ๋ฅผ ์ฌ์ฉํ๋ ๋ฐฉ์์ ์ฒ์ ๊ฐ์ฒด๋ฅผ ์์ฑํ  ๋๋ง ํ์ํ ํ๋ฆฌ๋ฏธํฐ๋ฅผ ๋๊ฒจ๋๊ณ , ์ปค๋ฅ์์ ํ๋ํ  ๋๋ ๋จ์ํ dataSource.getConnection() ๋ง ํธ์ถํจ.


#### ๐ <์ค์ต> ์ปค๋ฅ์ ํ์ ์ด๊ธฐ ์ค์ ํ๋ ์ฝ๋
```java
@Test  
void dataSourceConnectionPool() throws SQLException, InterruptedException {  
    //์ปค๋ฅ์ ํ๋ง: HikariProxyConnection(Proxy) -> JdbcConnection(Target)  
    HikariDataSource dataSource = new HikariDataSource();  
    dataSource.setJdbcUrl(URL);  
    dataSource.setUsername(USERNAME);  
    dataSource.setPassword(PASSWORD);  
    dataSource.setMaximumPoolSize(10);  
    dataSource.setPoolName("MyPool");  
    useDataSource(dataSource);  
    Thread.sleep(1000); //์ปค๋ฅ์ ํ์์ ์ปค๋ฅ์ ์์ฑ ์๊ฐ ๋๊ธฐ  
}
```

- ์ถ๋ ฅ
```
#์ปค๋ฅ์ ํ ์ด๊ธฐํ ์ ๋ณด ์ถ๋ ฅ
HikariConfig - MyPool - configuration:
HikariConfig - maximumPoolSize................................10
HikariConfig - poolName................................"MyPool"

#์ปค๋ฅ์ ํ ์ ์ฉ ์ฐ๋ ๋๊ฐ ์ปค๋ฅ์ ํ์ ์ปค๋ฅ์์ 10๊ฐ ์ฑ์
[MyPool connection adder] MyPool - Added connection conn0: url=jdbc:h2:.. 
user=SA
...
[MyPool connection adder] MyPool - Added connection conn9: url=jdbc:h2:.. 
user=SA

#์ปค๋ฅ์ ํ์์ ์ปค๋ฅ์ ํ๋1
ConnectionTest - connection=HikariProxyConnection@446445803 wrapping conn0: 
url=jdbc:h2:tcp://localhost/~/test user=SA, class=class 
com.zaxxer.hikari.pool.HikariProxyConnection

#์ปค๋ฅ์ ํ์์ ์ปค๋ฅ์ ํ๋2
ConnectionTest - connection=HikariProxyConnection@832292933 wrapping conn1: 
url=jdbc:h2:tcp://localhost/~/test user=SA, class=class 
com.zaxxer.hikari.pool.HikariProxyConnection
MyPool - After adding stats (total=10, active=2, idle=8, waiting=0)
```

- ๋งค์ปค๋์ฆ  
	- 1) ์ปค๋ฅ์ ํ **๊ตฌํ์ฒด ์์ฑ**  
	- 2) **๊ธฐ๋ณธ ์ค**์ (URL, NAME, PASSWORD, PoolSize)   
	- 3) **์ปค๋ฅ์ ํ์์ ์ฌ์ฉ**ํ๊ธฐ

- ์ปค๋ฅ์ ํ์์ ์ปค๋ฅ์์ ์์ฑํ๋ ์์์ ์ ํ๋ฆฌ์ผ์ด์ ์คํ ์๋์ ์ํฅ์ ์ฃผ์ง ์๊ธฐ ์ํด ๋ณ๋์ ์ฐ๋ ๋์์ ์๋ํจ.  


#### ๐ <์ค์ต> ๋น์ฆ๋์ค ๋ก์ง์ DataSource ์ ์ฉํ๋ ์ฝ๋
```java
/**  
 * JDBC - DataSource ์ฌ์ฉ, JdbcUtils ์ฌ์ฉ  
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

- J**dbcUtils ์ ์ฌ์ฉ**ํ๋ฉด ์ปค๋ฅ์์ ์ข ๋ ํธ๋ฆฌํ๊ฒ ๋ซ์ ์ ์๋ค.

- DriverManagerDataSource -> HikariDataSource ๋ก ๋ณ๊ฒฝํด๋ MemberRepositoryV1 ์ ์ฝ๋๋ ์ ํ ๋ณ๊ฒฝํ์ง ์์๋ ๋๋ค. 
 MemberRepositoryV1 ๋ DataSource **์ธํฐํ์ด์ค์๋ง ์์กด**ํ๊ธฐ ๋๋ฌธ์ด๋ค.


# ํธ๋์ญ์๊ณผ DB๋ฝ
## ํธ๋์ญ์ ์ดํด
#### ๐ ํธ๋์ญ์์ ์ฌ์ฉํ๋ ์ด์ ๋ ๋ญ๊น?
- ํ๋์ ๋ผ๋ฆฌ์ ์ธ ์์์ ๋จ์์ด๊ธฐ ๋๋ฌธ์,ย ์ฌ๋ฌ๊ฐ์ ์์์ ํ๋์ ๋ผ๋ฆฌ์ ์ธ ๋จ์๋ก ๋ฌถ์ด์ ๋ฐ์๊ณผ ๋ณต๊ตฌ๋ฅผ ์กฐ์ ํ  ์ ์๊ธฐ ์ํด ์ฌ์ฉ
- ์์) ๊ณ์ข์ด์ฒด ๋์ค ์ค๋ฅ๊ฐ ๋ฌ๋ค๋ฉด? ํธ๋์ญ์์ ์ฌ์ฉํ์ง ์์ ๊ฒฝ์ฐ ์ถ๊ธ์ ๋์ง๋ง ์๊ธ์ด ์๋๋ ๊ฒฝ์ฐ ๋ฐ์ํ  ์ ์์.

#### ๐ ํธ๋์ญ์ ACID
- ์์์ฑ  
    - ํธ๋์ญ์ ๋ด์์ ์คํํ ์์๋ค์ ๋ง์น **ํ๋์ ์์์ธ ๊ฒ์ฒ๋ผ** ๋ชจ๋ ์ฑ๊ณต ํ๊ฑฐ๋ ๋ชจ๋ ์คํจํด์ผํ๋ค.  
- ์ผ๊ด์ฑ  
    - ๋ชจ๋  ํธ๋์ญ์์ ์ผ๊ด์ฑ ์๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ํ๋ฅผ ์ ์งํด์ผ ํ๋ค. ์๋ฅผ ๋ค์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ์ ํ **๋ฌด๊ฒฐ์ฑ ์ ์ฝ ์กฐ๊ฑด**์ ํญ์ ๋ง์กฑํด์ผ ํ๋ค.  

- ๊ฒฉ๋ฆฌ์ฑ  
    - ๋์์ ์คํ๋๋ ํธ๋์ญ์๋ค์ด ์๋ก์๊ฒ ์ํฅ์ ๋ฏธ์น์ง ์๋๋ก ๊ฒฉ๋ฆฌํ๋ค. ์๋ฅผ ๋ค์ด ๋์์ ๊ฐ์ ๋ฐ์ดํฐ๋ฅผ ์์ ํ์ง ๋ชปํ๋๋ก ํด์ผ ํ๋ค. ๊ฒฉ๋ฆฌ์ฑ์ ๋์์ฑ๊ณผ ๊ด๋ จ๋ ์ฑ๋ฅ ์ด์๋ก ์ธํด ํธ๋์ญ์ ๊ฒฉ๋ฆฌ ์์ค (Isolation level)์ ์ ํํ  ์ ์๋ค.  
- ์ง์์ฑ  
    - ํธ๋์ญ์์ ์ฑ๊ณต์ ์ผ๋ก ๋๋ด๋ฉด ๊ทธ ๊ฒฐ๊ณผ๊ฐ ํญ์ ๊ธฐ๋ก๋์ด์ผ ํ๋ค. ์ค๊ฐ์ ์์คํ์ ๋ฌธ์ ๊ฐ ๋ฐ์ํด๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋ก๊ทธ ๋ฑ์ ์ฌ์ฉํด์ ์ฑ๊ณตํ ํธ๋์ญ์ ๋ด์ฉ์ ๋ณต๊ตฌํด์ผ ํ๋ค.


#### ๐  ๋ฌด๊ฒฐ์ฑ ์ ์ฝ์กฐ๊ฑด์ด๋? 
1) ๊ฐ์ฒด ๋ฌด๊ฒฐ์ฑ : ๊ธฐ๋ณธํค๋ null ๊ฐ์ด ๋ ์ ์์
2) ์ฐธ์กฐ ๋ฌด๊ฒฐ์ฑ : ์ธ๋ํค๋ ์ฐธ์กฐํ  ์ ์๋ ๊ฐ์ ๊ฐ์ง์ ์์
3) ๋๋ฉ์ธ ๋ฌด๊ฒฐ์ฑ : ํน์  ์์ฑ๊ฐ์ ๊ทธ ์์ฑ์ด ์ ์๋ ๋๋ฉ์ธ์ ์ํ ๊ฐ์ด์ด์ผํจ(enum๊ณผ ๊ฐ์ ๊ฐ๋)
4) ํค ๋ฌด๊ฒฐ์ฑ : ๋ฆด๋ ์ด์์๋ ์ต์ํ ํ๋์ ํค๊ฐ ์กด์ฌํด์ผํจ.
5) null ๋ฌด๊ฒฐ์ฑ : ํน์  ์์ฑ์ null๊ฐ์ ๊ฐ์ง ์  ์์
6) ๊ณ ์  ๋ฌด๊ฒฐ์ฑ : ํน์  ์์ฑ๊ฐ์ ์๋ก ๋ฌ๋ผ์ผํจ.
์ถ์ฒ : [[DB ๋ฐ์ดํฐ๋ฒ ์ด์ค] ๋ฌด๊ฒฐ์ฑ ์ ์ฝ์กฐ๊ฑด ](https://iingang.github.io/posts/DB-Integrity-constraint/)


#### ๐ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐ๊ฒฐ ๊ตฌ์กฐ์ DB ์ธ์ ์ค๋ช
![[Pasted image 20221203222930.png]]
- ์น ์ ํ๋ฆฌ์ผ์ด์ ์๋ฒ(WAS)๋ DB ์ ๊ทผ ํด ๊ฐ์ ํด๋ผ์ด์ธํธ๋ฅผ ์ฌ์ฉํด์ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์๋ฒ์ ์ ๊ทผํจ. ํด๋ผ์ธํธ์ ์๋ฒ๋ ์ญํ ์ ๊ฐ๋์ด๋ค!!  
- ๋ฐ์ดํฐ ๋ฒ ์ด์ค ์๋ฒ์ ์ปค๋ฅ์์ ํตํ **๋ชจ๋  ์์ฒญ์ ํด๋น ์ธ์**์ ํตํด์ ์คํํจ.  
- ์ธ์ ์ข๋ฃํ๋ ๋ฐฉ๋ฒ  
    - ํธ๋์ญ์์ ์์ํ๊ณ , ์ปค๋ฐ ๋๋ ๋กค๋ฐฑ์ ํตํด **ํธ๋์ญ์์ ์ข๋ฃ**ํ๋ค  
    - ์ฌ์ฉ์๊ฐ ์ปค๋ฅ์์ ๋ซ๊ฑฐ๋, ๋๋ DBA(DB ๊ด๋ฆฌ์)๊ฐ **์ธ์์ ๊ฐ์ ๋ก ์ข๋ฃ**ํ๋ฉด ์ธ์์ ์ข๋ฃ๋๋ค.
-  ์ธ์์ด๋? ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ ์์ ์์์ผ๋ก, ์ฌ๋ฌ ์์์ ์ํํ ํ ์ ์ ์ข๋ฃ๊น์ง์ ์ ์ฒด ๊ธฐ๊ฐ์ ์๋ฏธํ๋ค.
์ถ์ฒ : [[Oracle SQL] ํธ๋์ญ์, ์ธ์, ๋ฐ์ดํฐ ์ ์์ด(DDL)](https://developer-alle.tistory.com/203)


#### ๐<์ค์ต> ํธ๋์ญ์ ๊ณ์ข์ด์ฒด ์์
1) ๋๊ฐ์ ๋ค๋ฅธ ์ธ์์ด ๊ฐ์ ํ์ด๋ธ ๊ฐ์ ๊ฐ์ง๊ณ  ์๊ฒ ๋ง๋ค๊ธฐ(๊ธฐ๋ณธ์ค์ )
![[Pasted image 20221203225610.png]]
```SQL
//๋ฐ์ดํฐ ์ด๊ธฐํ
set autocommit true;
delete from member;
insert into member(member_id, money) values ('oldId',10000);
```


2) ์ ๊ท๋ฐ์ดํฐ ์ถ๊ฐํ๋ฉด ํด๋น ์ธ์๋ง ๋ฐ์ดํฐ ํ์ธ ๊ฐ๋ฅ
![[Pasted image 20221203225659.png]]
```SQL
//ํธ๋์ญ์ ์์
set autocommit false; //์๋ ์ปค๋ฐ ๋ชจ๋
insert into member(member_id, money) values ('newId1',10000);
insert into member(member_id, money) values ('newId2',10000);
```


3) if ์ปค๋ฐํ๋ฉด? ๋ค๋ฅธ ์ธ์์์๋ ํ์ ํ์ด๋ธ์ ์กฐํํ๋ฉด ์ ๊ท ํ์๋ค์ ํ์ธํ  ์ ์์.
![[Pasted image 20221203225738.png]]
```SQL
commit; //๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ฐ์
```

4) if ๋กค๋ฐฑํ๋ฉด? ๋ชจ๋ ํธ๋์ญ์์ ์์ํ๊ธฐ ์ง์ ์ ์ํ๋ก ๋ณต๊ตฌ๋จ.
![[Pasted image 20221203225812.png]]
```SQL
rollback; //๋กค๋ฐฑ์ผ๋ก ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ณ๊ฒฝ ์ฌํญ์ ๋ฐ์ํ์ง ์๋๋ค.
```

5) if ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ฉด
![[Pasted image 20221203230237.png]]
``` sql
set autocommit false;
update member set money=10000 - 2000 where member_id = 'memberA'; 
//์ฟผ๋ฆฌ ์์ธ๋ฐ์
update member set money=10000 + 2000 where member_iddd = 'memberB'; 
```
- memberA ์ ๋์ 2000์ ์ค์ด๋ค์์ง๋ง, memberB ์ ๋์ 2000์ ์ฆ๊ฐํ์ง ์์๋ค๋ ์ ์ด๋ค. ๊ฒฐ๊ณผ์ ์ผ๋ก ๊ณ์ข์ด์ฒด๋ ์คํจํ๊ณ  memberA ์ ๋๋ง 2000์ ์ค์ด๋  ์ํฉ.  
- ์ด๋ ๊ฐ์ ๋ก ์ปค๋ฐํ๋ฉด, ๋ฌธ์ ์ํฉ ์ ์ง  
- ๋กค๋ฐฑํ๋ฉด ์๋ ๋ฐ์ดํฐ๋ก ๋ณต๊ตฌ


#### ๐ ์ปค๋ฐํ์ง ์์ ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฅธ ๊ณณ์์ ์กฐํํ  ์ ์์ผ๋ฉด ์ด๋ค ๋ฌธ์ ๊ฐ ๋ฐ์ํ ๊น?
- ๋ฐ์ดํฐ ์ ํฉ์ฑ์ ํฐ ๋ฌธ์ ๊ฐ ๋ฐ์  
- ์) A์ธ์์ด ์์ ์ํ์ ์๋ ๋ฐ์ดํฐ ๋ง๋ค์๋ค. ํด๋น ๋ฐ์ดํฐ๋ฅผ B์ธ์์ด ๋ฐ์ดํฐ๋ฅผ ๋ณ๊ฒฝํ๊ณ  ์ ์ฅํ์ง๋ง, A์ธ์์ด ๋กค๋ฐฑํ๋ฉด ๋ฐ์ดํฐ ์ฌ๋ผ์ง.


#### ๐ ์ ํฉ์ฑ๊ณผ ๋ฌด๊ฒฐ์ฑ์ ์ฐจ์ด๋?
- ์ ํฉ์ฑ์ด๋? ์ด๋ค ๋ฐ์ดํฐ๋ค์ด ๊ฐ์ด ์๋ก ์ผ์นํ๋ ์ํ๋ฅผ ์๋ฏธ
- ๋ฌด๊ฒฐ์ฑ์ด๋? ๋ฐ์ดํฐ ๊ฐ์ด ์ ํํ ์ํ
- ์ ํฉ์ฑ O, ๋ฌด๊ฒฐ์ฑ X ์์
	์ฃผ๋ฌธ์ ๋ณด ํ์ด๋ธ์์ ๊ณ ๊ฐ๋ฒํธ๊ฐ ๋ชจ๋ -1์ผ๋ก ์๋ ฅ๋์ด ์๊ณ ,  ๊ณ ๊ฐ์ ๋ณด ํ์ด๋ธ์๋ -1์ ๊ฐ์ ๊ฐ๋ ๊ณ ๊ฐ์ด ์กด์ฌํ๋ค. (๋ฐ์ดํฐ์ ๊ฐ์ด ์ผ์นํ๋ค.) ๊ทธ๋ฌ๋ ๊ณ ๊ฐ๋ฒํธ๋ ๋ฐ๋์ 1 ์ด์์ ๊ฐ์ ๊ฐ์ ธ์ผ ํ๋ค. (๋ฐ์ดํฐ์ ๊ฐ์ด ์ ํํ์ง ์๋ค.) ์ด๋ฐ ์ํฉ์๋ ๋ฐ์ดํฐ ์ ํฉ์ฑ์ ์ด์์ด ์์ผ๋, ๋ฐ์ดํฐ ๋ฌด๊ฒฐ์ฑ์ ํผ์๋์๋ค๊ณ  ๋ณผ ์ ์๋ค.
- ์ ํฉ์ฑ X, ๋ฌด๊ฒฐ์ฑ O ์์
	์ ์์์์ ์ฃผ๋ฌธ์ ๋ณด ํ์ด๋ธ์ ๊ณ ๊ฐ๋ฒํธ๋ฅผ -1์์ 2๋ก ๋ณ๊ฒฝํ์ง๋ง, ๊ณ ๊ฐ์ ๋ณด ํ์ด๋ธ์๋ ๊ณ ๊ฐ๋ฒํธ๊ฐ ๋ณ๊ฒฝ๋์ง ์์์ ๋, (๋ฐ์ดํฐ์ ๊ฐ์ด ์๋ก ์ผ์นํ์ง ์๋๋ค.)  ๋ฐ์ดํฐ ์ ํฉ์ฑ์ด ํผ์๋์๋ค๊ณ  ๋ณผ ์ ์๋ค.
์ถ์ฒ : [๋ฌด๊ฒฐ์ฑ๊ณผ ์ ํฉ์ฑ์ด๋ ๋ฌด์์ธ๊ฐ? (velog.io)](https://velog.io/@yangsijun528/%EB%AC%B4%EA%B2%B0%EC%84%B1%EA%B3%BC-%EC%A0%95%ED%95%A9%EC%84%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)


#### ๐ ์๋์ปค๋ฐ VS ์๋ ์ปค๋ฐ
- ์๋ ์ปค๋ฐ ๋ชจ๋๋ ์๋ ์ปค๋ฐ ๋ชจ๋๋ ํ๋ฒ ์ค์ ํ๋ฉด ํด๋น ์ธ์์์๋ ๊ณ์ ์ ์ง๋๋ค. ์ค๊ฐ์ ๋ณ๊ฒฝํ๋ ๊ฒ์ ๊ฐ๋ฅ

- ์๋ ์ปค๋ฐ ์ค์ 
``` SQL
set autocommit true; //์๋ ์ปค๋ฐ ๋ชจ๋ ์ค์ 
insert into member(member_id, money) values ('data1',10000);
insert into member(member_id, money) values ('data2',10000);
```
์ฅ์  : ์ปค๋ฐ์ด๋ ๋กค๋ฐฑ์ ์ง์  ํธ์ถํ์ง ์์๋ ๋๋ ํธ๋ฆฌํจ.
๋จ์  : ์ํ๋ ํธ๋์ญ์ ๊ธฐ๋ฅ์ ์ ๋๋ก ์ฌ์ฉํ  ์ ์์.

- ์๋ ์ปค๋ฐ ์ค์ 
``` SQL
set autocommit false;
insert into member(member_id, money) values ('data1',10000);
insert into member(member_id, money) values ('data2',10000);
commit; //์๋ ์ปค๋ฐ
```
์ฅ์  : ํธ๋์ น์ ๊ธฐ๋ฅ ์ฌ์ฉ.
๋จ์  : ์ปค๋ฐ/๋กค๋ฐฑ์ ์ผ์ผํ ์ณ์ค์ผํจ.

## DB ๋ฝ
#### ๐ DB ๋ฝ ๊ฐ๋ ์ค๋ช
- DB๋ฝ์ด๋? ์ธ์์ด ํธ๋์ญ์์ ์์ํ๊ณ  ๋ฐ์ดํฐ๋ฅผ ์์ ํ๋ ๋์์๋ ์ปค๋ฐ์ด๋ ๋กค๋ฐฑ ์ ๊น์ง ๋ค๋ฅธ ์ธ์์์ ํด๋น ๋ฐ์ดํฐ๋ฅผ ์์ ํ  ์ ์๊ฒ ๋ง์์ผ ํ๋ค. why? ํธ๋์ญ์์ ์์์ฑ์ด ๊นจ์ง.
- ์ผ๋ฐ์ ์ธ ์กฐํ์๋ ๋ฐ์ดํฐ๊ฐ ๋ณ๊ฒฝ๋์ง ์๊ธฐ ๋๋ฌธ์  DB๋ฝ์ ์ฌ์ฉ X . ๊ทธ๋ฌ๋ ๋ฝ์ ํ๋ํ๊ณ  ์ถ๋ค๋ฉด `for update` ๊ตฌ๋ฌธ์ฌ์ฉ
```
select * from member where member_id='memberA' for update;
```
- ์ค๋์๊ฐ ๋ฝ์ ํ๋ํ์ง ๋ชปํ๋ฉด ํ์ ์์ ์ค๋ฅ ๋ฐ์ํจ.
![[Pasted image 20221204000916.png]]


#### ๐<์ค์ต> DB ๋ฝ ๋ณ๊ฒฝ ์์
1) ๊ธฐ๋ณธ ๋ฐ์ดํฐ๋ฅผ ์๋ ฅํ์.
![[Pasted image 20221203231516.png]]
```sql
set autocommit true;  //์๋ ์ปค๋ฐ ์ค์ 
delete from member;  //์ด๊ธฐํ
insert into member(member_id, money) values ('memberA',10000);
```

2) ์ธ์1์ ํธ๋์ญ์์ ์์ํ๋ค. ๋ฝ์ด ๋จ์ ์์ผ๋ฏ๋ก ์ธ์1์ ๋ฝ์ ํ๋ํ๋ค.
![[Pasted image 20221203231528.png]]
```sql
set autocommit false;
update member set money=500 where member_id = 'memberA';
```

3) ์ธ์1์ด ์ปค๋ฐ์ ํ๋ค. ์ปค๋ฐ์ผ๋ก ํธ๋์ญ์์ด ์ข๋ฃ๋์์ผ๋ฏ๋ก ๋ฝ๋ ๋ฐ๋ฉํ๋ค.
![[Pasted image 20221203231534.png]]
```sql
SET LOCK_TIMEOUT 60000;
set autocommit false;
update member set money=1000 where member_id = 'memberA';
```

4) ์ธ์1์ด ์ปค๋ฐ์ ํ๋ค. ์ปค๋ฐ์ผ๋ก ํธ๋์ญ์์ด ์ข๋ฃ๋์์ผ๋ฏ๋ก ๋ฝ๋ ๋ฐ๋ฉํ๋ค.
![[Pasted image 20221203231657.png]]
```sql
commit;
```

5) ํ์ ์์์ , ์ธ์2๊ฐ ๋ฝ์ ํ๋ํ๋ค. update sql์ ์ํํ๋ค.
![[Pasted image 20221203231705.png]]
![[Pasted image 20221203231709.png]]

6) ์ธ์2๋ ์ปค๋ฐ์ ์ํํ๊ณ  ํธ๋์ญ์์ด ์ข๋ฃ๋์์ผ๋ฏ๋ก ๋ฝ์ ๋ฐ๋ฉํ๋ค.
![[Pasted image 20221203231726.png]]
```sql
commit;
```

## ๋ก์ง๊ณผ ํธ๋์ญ์
#### ๐ <์ค์ต>ํธ๋์ญ์ ์์ด Service ๊ณ์ธต ๊ตฌํ
- ํธ๋์ญ์ ์๋ ๊ณ์ข์ด์ฒด ์์
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
            throw new IllegalStateException("์ด์ฒด์ค ์์ธ ๋ฐ์");  
        }  
    }  
}
```


#### ๐ Repository์ Service ๊ณ์ธต์ ์ฐจ์ด?
- Service ๊ณ์ธต
Controller์ Dao์ ์ค๊ฐ ์์ญ์์ ์ฌ์ฉํจ.
Repository์ ๋ฉ์๋๋ฅผ ์ฌ๋ฌ ๊ฐ ์ฌ์ฉํด ๋น์ฆ๋์ค ๋ก์ง์ ๊ตฌ์ฑํจ.

- Repository ๊ณ์ธต
Database ์ ๊ฐ์ด ๋ฐ์ดํฐ ์ ์ฅ์์ ์ ๊ทผํ๋ ์์ญ์.
CRUD๋ฅผ ํตํด ๋ฐ์ดํฐ์ ์ ๊ทผํจ.

์ถ์ฒ : [์คํ๋ง ํ๋ก์ ํธ ๊ณ์ธต ๊ตฌ์กฐ ์ดํด](https://blog.naver.com/ghdalswl77/222489613651)


#### ๐ ํธ๋์ญ์์ ์ด๋ค ๊ณ์ธต์ ๊ฑธ์ด์ผ ํ ๊น?
![[Pasted image 20221204001320.png]]
- ํธ๋์ญ์์ ๋น์ฆ๋์ค ๋ก์ง์ด ์๋ **์๋น์ค ๊ณ์ธต์์ ์์**ํด์ผ ํ๋ค. why? ๋น์ฆ๋์ค ๋ก์ง์ผ๋ก ์ธํด ๋ฌธ์ ๊ฐ ๋๋ ๋ถ๋ถ์ ํจ๊ป ๋กค๋ฐฑํด์ผ ํ๊ธฐ ๋๋ฌธ์ด๋ค.  
- **ํธ๋์ญ์์ ์์ํ๋ ค๋ฉด ์ปค๋ฅ์์ด ํ์**ํจ. ๊ทธ๋ฆฌ๊ณ  **ํธ๋์ญ์์ ์ฌ์ฉํ๋ ๋์ ๊ฐ์ ์ปค๋ฅ์์ ์ ์ง**ํด์ผํจ. HOW? ํ๋ผ๋ฏธํฐ๋ก ์ปค๋ฅ์์ ์ ๋ฌํ์ฌ ์ฌ์ฉํ์!!


#### ๐ <์ค์ต> ํธ๋์ญ์ ์ถ๊ฐํ ์ฝ๋
- Respositoy ๊ณ์ธต ํธ๋์ญ์ ์ถ๊ฐ
```java
...
public Member findById(Connection con, String memberId) throws SQLException {  
    String sql = "select * from member where member_id = ?";  
  
    PreparedStatement pstmt = null;  
    ResultSet rs = null;  
  
    try {  
	    //con = getConnection(); ์์ด์ง
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
        //connection์ ์ฌ๊ธฐ์ ๋ซ์ง ์๋๋ค.  
        JdbcUtils.closeResultSet(rs);  
        JdbcUtils.closeStatement(pstmt);  
        //JdbcUtils.closeConnection(con); ์์ด์ง
    }  
  
}
...
```
- ๊ฐ์ ์ปค๋ฅ์์ ์ ์งํ๊ธฐ ์ํด ์๋กญ๊ฒ ํจ์ ์ ์ํด์ผํจ. ํ๋ผ๋ฏธํฐ๋ก ์ปค๋ฅ์ ๋๊ฒจ์ฃผ๊ณ  `con = getConnection()` ๋ฅผ ์์ค๋ค. why? ์๋ก์ด ์ปค๋ฅ์์ ํ๋ํ์ง ์๊ณ  ํ๋ผ๋ฏธํฐ๋ก ๋๊ฒจ ๋ฐ์๊ฑฐ๊ธฐ ๋๋ฌธ์ด๋ค.  
- finally์์ ์ปค๋ฅ์ ์ข๋ฃํ์ง ๋ง๊ณ , ์๋น์ค ๋ก์ง์์ ์ข๋ฃํ๊ธฐ
- findById์ ๊ฐ์ด ๋ค๋ฅธ ๋ฉ์๋(update)๋ ๋ณ๊ฒฝํด์ฃผ๊ธฐ


- Service ๊ณ์ธต์ ํธ๋์ญ์ ์ถ๊ฐ
```java
@RequiredArgsConstructor  
public class MemberServiceV2 {  
    private final DataSource dataSource;  
    private final MemberRepositoryV2 memberRepository;  

    public void accountTransfer(String fromId, String toId, int money) throws SQLException {  
	    //์ด์  ์ฌ๊ธฐ์ ์ปค๋ฅ์์ ์ป์ด์ผ ํด์ ์ถ๊ฐ
        Connection con = dataSource.getConnection(); 
         
        try {  
            con.setAutoCommit(false);//ํธ๋์ญ์ ์์  
            //๋น์ฆ๋์ค ๋ก์ง  
            bizLogic(con, fromId, toId, money);  
            con.commit(); //์ฑ๊ณต์ ์ปค๋ฐ  
            
        } catch (Exception e) {  
            con.rollback(); //์คํจ์ ๋กค๋ฐฑ  
            throw new IllegalStateException(e); 
             
        } finally {  
            release(con);  
        }  
  
    }  

...

	/*๋ฆฌ์์ค ์ ๋ฆฌ*/
    private void release(Connection con) {  
        if (con != null) {  
            try {  
                con.setAutoCommit(true); //๊ธฐ๋ณธ๊ฐ์ผ๋ก ์ปค๋ฅ์ ํ ๊ณ ๋ ค  
                con.close();  
            } catch (Exception e) {  
                log.info("error", e);  
            }  
        }  
    }
}
```
- Connection con = dataSource.getConnection() ์ผ๋ก ํ๋์ ์ปค๋ฅ์ ์ ์งํ๊ธฐ
- con.setAutoCommit(false) ์ผ๋ก ํธ๋์ญ์ ์์
- release(con) ์ผ๋ก ๊ธฐ๋ณธ ๊ฐ์ธ ์๋ ์ปค๋ฐ ๋ชจ๋๋ก ๋ณ๊ฒฝํ๋ ๊ฒ + ์ปค๋ฅ์์ ๋ฐ๋ฉํ๊ธฐ

#### ๐ 


#### ๐ 


#### ๐ 


#### ๐ 


#### ๐ 


#### ๐ 


#### ๐ 