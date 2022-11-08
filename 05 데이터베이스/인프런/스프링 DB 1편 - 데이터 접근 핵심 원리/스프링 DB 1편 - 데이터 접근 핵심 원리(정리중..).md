-   JDBC 이해  
    
    -   JDBC 이해  
        
        -   데이터 베이스 쓰는 이유는?  
            -   애플리케이션을 개발할 때 중요한 데이터는 대부분 데이터베이스에 보관함.  
                자바코드에 보관하면, 자바가 종료되면 데이터가 날라감.
        
        -   서버와 DB 사용법은?  
            
            -   동작 과정 그림으로 이해![](https://api.transno.com/v3/document_image/65e3a51b-9e3b-440b-8e19-7d17720a50b4-10826299.jpg)  
                
            
            -   매커니즘  
                
                -   1. 커넥션 연결: 주로 TCP/IP를 사용해서 커넥션을 연결한다.  
                    
                
                -   2. SQL 전달: 애플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.  
                    
                
                -   3. 결과 응답: DB는 전달된 SQL을 수행하고 그 결과를 응답한다. 애플리케이션 서버는 응답 결과를 활용한다.  
                    
        
        -   JDBC의 2가지 문제점  
            
            -   1) 다른 종류의 데이터베이스로 변경하면 애플리케이션 서버에 개발된 데이터베이스 사용코드도 함께 변경해야 한다.  
                
            
            -   2) 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 그리고 그 결과를 응답 받는 방법을 새로 학습해야 한다.  
                
            
            -   즉, 데이터 베이스마다 통합 X  
                
        
        -   JDBC 표준 인터페이스의 해결방법은?  
            
            -   첫번째 문제 해결을 위해, 애플리케이션 로직은 이제 JDBC 표준 인터페이스에만 의존한다.  
                따라서 데이터베이스를 다른 종류의 데이터베이스로 변경하고 싶으면 JDBC 구현 라이브러리만 변경하면 된다.
            
            -   두번째 문제 해결을 위해, JDBC 표준 인터페이스 사용법만 학습하면 된다.  
                한번 배워두면 수십개의 데이터베이스에 모두 동일하게 적용할 수 있다.
        
        -   JDBC 표준 인터페이스 동작 방법은?  
            
            -   인터페이스 구현![](https://api.transno.com/v3/document_image/6a063c67-f38d-4dd7-b5b1-e93989cf015e-10826299.jpg)  
                
            
            -   기존 방식과 연결하는 방법은 동일함.![](https://api.transno.com/v3/document_image/f3e0c00d-1b24-4029-af7c-fef24a5f740e-10826299.jpg)  
                
    
    -   JDBC와 최신 데이터 접근 기술  
        
        -   최신 기술을 왜 써야할까?  
            
            -   사용하기에 복잡함  
                
            
            -   JDBC 사용하는 경우 로직![](https://api.transno.com/v3/document_image/a95ddd59-f7c2-47a4-b9a0-cb8e97d649ca-10826299.jpg)  
                
        
        -   SQL Mapper  
            
            -   로직은?![](https://api.transno.com/v3/document_image/9bfe669a-5382-4c15-85d7-a861ca40357d-10826299.jpg)  
                객체와 관계형 데이터베이스의 데이터를 개발자가 작성한 SQL로 매핑시켜주는 프레임워크
            
            -   장점  
                
                -   SQL 응답 결과를 객체로 편리하게 변환해준다.  
                    
                
                -   JDBC의 반복 코드를 제거해준다.  
                    
            
            -   단점  
                -   개발자가 SQL을 직접 작성해야한다.  
                    
        
        -   ORM 기술  
            
            -   로직은?![](https://api.transno.com/v3/document_image/1b23e098-228a-4892-a067-de298fe027bb-10826299.jpg)  
                객체와 관계형 데이터베이스의 데이터를 자동으로 매핑시켜주는 프레임워크
            
            -   장점  
                
                -   재사용 유지 보수 편리성이 증가 한다.  
                    
                
                -   쿼리에 집중할 필요 없이 빠르게 개발이 가능하다  
                    
                
                -   객체 지향적인 코드로 직관적인 비지니스 로직을 작성할 수 있다.  
                    
            
            -   단점  
                
                -   초기 적용에 러닝 커브가 어느 정도 있다.  
                    
                
                -   복잡한 SQL의 사용하지 못할 수 도 있다.  
                    
    
    -   데이터베이스 연결  
        
        -   코드로 이해하기  
            
            -   ConnectionConst  
                -   접속 기본정보 기입![](https://api.transno.com/v3/document_image/696e2255-e4b5-42af-970f-3435a346fcd7-10826299.jpg)  
                    접속하는데 필요한 기본 정보를 편리하게 사용할 수 있도록 상수만들기.
            
            -   DBConnectionUtil  
                -   데이터베이스에 연결 코드![](https://api.transno.com/v3/document_image/157cafdf-7b4d-4730-a355-f81c03c4468a-10826299.jpg)  
                    JDBC를 사용해서 실제 데이터베이스에 연결하는 코드 만들기. H2 데이터베이스 드라이버가 작동해서 실제 데이터베이스와 커넥션을 맺고 그 결과를 반환해준다.(자동으로 데이터베이스 찾음)
        
        -   JDBC DriverManager 연결 이해  
            -   요청 흐름![](https://api.transno.com/v3/document_image/e514da8c-c3fe-460a-acf1-77b4430c33f3-10826299.jpg)  
                
                -   1. 애플리케이션 로직에서 커넥션이 필요하면 DriverManager.getConnection() 을 호출  
                    why? 해당 데이터 베이스 연결하기 위함
                
                -   2. DriverManager 는 라이브러리에 등록된 드라이버 목록을 자동으로 인식한다. 이 드라이버들에게 순서대로 다음 정보(URL, ID, PASSWORD)를 넘겨서 커넥션을 획득할 수 있는지 확인.  
                    
                
                -   3. 찾은 커넥션 구현체가 클라이언트에 반환된다.  
                    
    
    -   JDBC 개발 - 등록  
        -   코드로 이해하기  
            
            -   SQL에 테이블 만들기  
                -   ![](https://api.transno.com/v3/document_image/6aa4fb4a-1641-4690-990f-c154682be4c3-10826299.jpg)  
                    테이블과 샘플 데이터 만들기를 통해서 member 테이블을 미리 만들어두어야 한다.
            
            -   Member  
                -   코드![](https://api.transno.com/v3/document_image/21ae74ad-1d8f-4bd9-a185-141469404c26-10826299.jpg)  
                    회원 객체 만들기
            
            -   MemberRepositoryV0  
                -   등록![](https://api.transno.com/v3/document_image/66b623f2-f349-4de0-a5ea-6cfab42cd0bc-10826299.jpg)![](https://api.transno.com/v3/document_image/3d6a9a02-4bc2-4c53-8080-6b4c30ee593c-10826299.jpg)  
                    회원 객체를 데이터 베이스에 등록.
            
            -   매커니즘 설명  
                
                -   1) 전달할 SQL 정의함  
                    
                
                -   2) 2가지 준비하기  
                    
                    -   데이터베이스에 커넥션 준비. (Connetion)과 전달할 SQL  
                        
                    
                    -   PreparedStatement -> (1) DB에 전달할 SQL과 (2)파라미터로 전달할 데이터들을 준비함.  
                        
                
                -   3) 커넥션 연결은 con, 파라미터 전달은 pstmt를 통해 바인딩함.  
                    -   바인딩이란 오브젝트의 프로퍼티(메서드 set)에 값을 넣는 것을 말한다.  
                        
                
                -   4) SQL을 커넥션을 통해 실제 데이터베이스에 전달한다.  
                    
                
                -   5) 리소스 정리 역순으로 하기  
                    
    
    -   JDBC 개발 - 조회  
        -   코드로 이해하기  
            
            -   MemberRepositoryV0  
                -   조회![](https://api.transno.com/v3/document_image/cb42375d-360c-463a-a6a6-2f1a990f0260-10826299.jpg)  
                    회원 객체를 데이터베이스에 저장
            
            -   매커니즘 설명  
                
                -   1) 전달할 SQL 정의함.  
                    
                
                -   2) 3가지 준비하기  
                    
                    -   데이터베이스에 커넥션 준비. (Connetion)  
                        
                    
                    -   PreparedStatement -> (1) DB에 전달할 SQL과 (2)파라미터로 전달할 데이터들을 준비함.  
                        -   서버에서 DB로 넣는 input  
                            
                    
                    -   ResultSet -> select 쿼리의 결과가 순서대로 들어간다  
                        
                        -   DB에서 서버로 나오는 output  
                            
                        
                        -   예시) select member_id, money 라고 지정하면 member_id , money 라는 이름으로 데이터가 저장함.  
                            
                
                -   3) SQL을 커넥션을 통해  
                    
                
                -   4) rs. next -> if/while을 통해 커서 이동  
                    
                
                -   5) 빈칸이면, 새로운 회원 객체만들고, 상태값 넣기 후 리턴  
                    
                
                -   6) 리소스 정리 역순으로 하기  
                    
            
            -   보충 설명  
                
                -   [ExecuteQuery, ExecuteUpdate 차이점](https://mozi.tistory.com/26)  
                    
                    -   ExecuteQuery -> 수행결과로 ResultSet 객체의 값을 반환  
                        
                    
                    -   ExecuteUpdate -> 수행결과로 Int 타입의 값을 반환  
                        
                
                -   rs .next는 왜 있지?  
                    
                    -   rs는 현재 커서의 위치를 나타낸다고 생각하면 됌.  
                        
                    
                    -   데이터베이스 찾는 흐름![](https://api.transno.com/v3/document_image/229f040e-4217-4044-8353-3a37b98b5b75-10826299.jpg)  
                        rs. next를 통해서 커서가 이동함(if는 단일, while 복수 일때)
    
    -   JDBC 개발 - 수정, 삭제  
        -   코드로 이해하기  
            
            -   수정![](https://api.transno.com/v3/document_image/5f33fee7-8f11-4fcc-8202-88e51c68ce7e-10826299.jpg)  
                
            
            -   삭제![](https://api.transno.com/v3/document_image/33525a94-fe2a-4b7d-bd4b-720b879ddc20-10826299.jpg)  
                
    
    -   중요내용  
        
        -   서버와 DB  
            
        
        -   JDBC 표준 인터페이스 동작 방법  
            
        
        -   connection/PreparedStatement/ResultSet가 하는 역할  
            
-   커넥션풀과 데이터소스 이해  
    
    -   커넥션 풀 이해  
        
        -   커넥션 풀을 사용하기전에 문제점  
            
            -   복잡한 과정 + 시간 소모가 많음  
                
            
            -   데이터베이스 커넥션을 매번 회득![](https://api.transno.com/v3/document_image/c0728d72-82e2-46cc-87ac-7dcba285e572-10826299.jpg)  
                
            
            -   매커니즘  
                
                -   1. 애플리케이션 로직은 DB 드라이버를 통해 커넥션을 조회한다.  
                    
                
                -   2. DB 드라이버는 DB와 TCP/IP 커넥션을 연결한다. 물론 이 과정에서 3 way handshake 같은 TCP/IP 연결을 위한 네트워크 동작이 발생한다.  
                    
                
                -   3. DB 드라이버는 TCP/IP 커넥션이 연결되면 ID, PW와 기타 부가정보를 DB에 전달한다.  
                    
                
                -   4. DB는 ID, PW를 통해 내부 인증을 완료하고, 내부에 DB 세션을 생성한다.  
                    
                
                -   5. DB는 커넥션 생성이 완료되었다는 응답을 보낸다.  
                    
                
                -   6. DB 드라이버는 커넥션 객체를 생성해서 클라이언트에 반환한다.  
                    
        
        -   커넥션 풀 사용  
            
            -   커넥션 풀 사용![](https://api.transno.com/v3/document_image/86b4ac71-a69c-4924-ae11-79cfa91c0e6e-10826299.jpg)![](https://api.transno.com/v3/document_image/81bb889d-31c5-4d36-aaaf-ce2d9e69b557-10826299.jpg)  
                
            
            -   매커니즘  
                
                -   1. 커넥션 풀을 통해 이미 생성되어 있는 커넥션을 객체 참조 조회하기  
                    
                
                -   2. 커넥션 풀은 자신이 가지고 있는 커넥션 중에 하나를 반환한다.  
                    
                
                -   3. 애플리케이션 로직은 커넥션 풀에서 받은 커넥션을 사용해서 SQL을 데이터베이스에 전달하고 그 결과를 받아서 처리하기  
                    
                
                -   4. 다음에 다시 사용할 수 있도록 해당 커넥션을 그대로 커넥션 풀에 반환하면 된다. (종료 X)  
                    
        
        -   커넥션 풀 사용 장점  
            
            -   DB에 무한정 연결이 생성되는 것을 막아주어서 DB를 보호하는 효과가 있음  
                
            
            -   커넥션을 새로 만드는 시간이 없기 때문에 응답 속도를 빠르게함.  
                
    
    -   DataSource 이해  
        
        -   정의  
            -   커넥션 풀을 추상화한것  
                
        
        -   DriverManager 를 통해서 커넥션을 획득하다가, 커넥션 풀을 사용하는 방법으로 변경하려면 어떻게 해야할까?  
            -   추상화![](https://api.transno.com/v3/document_image/031c952e-3ae0-41a1-b77a-3672dbf1c12a-10826299.jpg)  
                -   DataSource 인터페이스에만 의존하도록 애플리케이션 로직을 작성  
                    
    
    -   DataSource 예제 1 - DriveManager  
        
        -   코드로 보기  
            
            -   DriverManager![](https://api.transno.com/v3/document_image/b27eb2d3-c5a2-442f-99e6-2d435da022d9-10826299.jpg)  
                
            
            -   실행결과![](https://api.transno.com/v3/document_image/3a62f5ab-e35d-4a75-8e96-5ae12564d5b2-10826299.jpg)  
                
            
            -   DriverManagerDataSource![](https://api.transno.com/v3/document_image/740a49a9-667f-4dcc-9009-4cfdf8fd0eb3-10826299.jpg)  
                
            
            -   실행 결과![](https://api.transno.com/v3/document_image/27df07f8-1213-485c-8b94-e8534c9f1d1a-10826299.jpg)  
                
        
        -   DriverManager와 DriverManagerDataSource 둘은 무슨 차이가 있지?  
            -   설정과 사용의 분리했음.  
                
                -   DriverManager 는 커넥션을 획득할 때 마다 URL , USERNAME , PASSWORD 같은 파라미터를 계속 전달함.  
                    
                
                -   DataSource 를 사용하는 방식은 처음 객체를 생성할 때만 필요한 파리미터를 넘겨두고, 커넥션을 획득할 때는 단순히 dataSource.getConnection() 만 호출함.  
                    
    
    -   DataSource 예제 2 - 커넥션풀  
        
        -   코드로 보기  
            
            -   HikariCP![](https://api.transno.com/v3/document_image/c9d060ba-e558-4e3d-a95c-e3c22f1b8b82-10826299.jpg)  
                
            
            -   결과![](https://api.transno.com/v3/document_image/1fcd65bf-9519-444b-b652-cbd9b1ad1bfa-10826299.jpg)  
                
        
        -   커넥션 풀에서 커넥션을 생성하는 작업은 애플리케이션 실행 속도에 영향을 주지 않기 위해 별도의 쓰레드에서 작동함.  
            -   why? 생성은 시간이 오래걸림  
                
        
        -   매커니즘  
            
            -   1) 커넥션 풀 구현체 생성  
                
            
            -   2) 기본 설정(URL, NAME, PASSWORD, PoolSize)  
                
            
            -   3) 커넥션 풀에서 사용하기  
                
    
    -   DataSource 적용  
        -   코드로 보기![](https://api.transno.com/v3/document_image/5f498418-c8ea-4dc9-8d27-304b34343a1d-10826299.jpg)  
            JdbcUtils 을 사용하면 커넥션을 좀 더 편리하게 닫을 수 있다.
            -   DriverManagerDataSource -> HikariDataSource 로 변경해도 MemberRepositoryV1 의 코드는 전혀 변경하지 않아도 된다. MemberRepositoryV1 는 DataSource 인터페이스에만 의존하기 때문이다.  
                
    
    -   중요 내용  
        
        -   DataSource 정의와 왜 쓰는거지?  
            
        
        -   DataSource 쓰기 전후 매커니즘 차이는?  
            
-   트랜잭션 이해  
    
    -   트랜잭션 - 개념 이해  
        
        -   단순히 파일에 저장해도 되는데, 데이터베이스에 저장하는 이유는 무엇일까?  
            -   트랜잭션 : 하나의 거래를 안전하게 처리하도록 보장해주는 것  
                
        
        -   트랜잭션 ACID  
            
            -   원자성  
                -   트랜잭션 내에서 실행한 작업들은 마치 하나의 작업인 것처럼 모두 성공 하거나 모두 실패해야한다.  
                    
            
            -   일관성  
                
                -   모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다. 예를 들어 데이터베이스에서 정한 무결성 제약 조건을 항상 만족해야 한다.  
                    
                
                -   [무결성 제약조건이란?](https://kosaf04pyh.tistory.com/202)  
                    
            
            -   격리성  
                -   동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지 않도록 격리한다. 예를 들어 동시에 같은 데이터를 수정하지 못하도록 해야 한다. 격리성은 동시성과 관련된 성능 이슈로 인해 트랜잭션 격리 수준 (Isolation level)을 선택할 수 있다.  
                    
            
            -   지속성  
                -   트랜잭션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템에 문제가 발생해도 데이터베이스 로그 등을 사용해서 성공한 트랜잭션 내용을 복구해야 한다.  
                    
        
        -   트랜잭션 격리 수준은 보통 READ COMMITTED  
            
    
    -   데이터베이스 연결 구조와 DB 세션  
        -   서버와 데이터베이스 연결 그림  
            
            -   그림![](https://api.transno.com/v3/document_image/f4575783-b94f-473e-8b4b-376e9103e9d9-10826299.jpg)![](https://api.transno.com/v3/document_image/cb0d64a0-4241-4208-afb7-2f5b678099d9-10826299.jpg)  
                
            
            -   웹 애플리케이션 서버(WAS)나 DB 접근 툴 같은 클라이언트를 사용해서 데이터베이스 서버에 접근  
                -   클라와 서버는 역할의 개념이다!!  
                    
            
            -   데이터 베이스 서버은 커넥션을 통한 모든 요청은 해당 세션을 통해서 실행함.  
                
            
            -   세션 종료하는 방법  
                
                -   트랜잭션을 시작하고, 커밋 또는 롤백을 통해 트랜잭션을 종료한다  
                    
                
                -   사용자가 커넥션을 닫거나, 또는 DBA(DB 관리자)가 세션을 강제로 종료하면 세션은 종료된다.  
                    
            
            -   [세션 보충 설명](https://okky.kr/article/680128?note=1906506)  
                -   방문자가 웹서버에 접속해 있는 상태를 하나의 단위로 보고 세션이라고 칭한다  
                    
    
    -   트랜잭션 - DB 예제1 - 개념 이해  
        
        -   용어 정리  
            
            -   커밋  
                -   데이터베이스에 결과를 반영하려는 명령어  
                    
            
            -   롤백  
                -   데이터베이스에 결과를 반영하지 않을려는 명령어  
                    
            
            -   정합성  
                -   어떤 데이터들이 값이 서로 일치하는 상태를 의미  
                    
            
            -   무결성  
                -   데이터 무결성이란 데이터 값이 정확한 상태를 의미  
                    
            
            -   정합성과 무결성의 차이는?  
                -   [무결성과 정합성이란 무엇인가? (velog.io)](https://velog.io/@yangsijun528/%EB%AC%B4%EA%B2%B0%EC%84%B1%EA%B3%BC-%EC%A0%95%ED%95%A9%EC%84%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)  
                    
        
        -   커밋하지 않은 데이터를 다른 곳에서 조회할 수 있으면 어떤 문제가 발생할까?  
            
            -   데이터 정합성에 큰 문제가 발생  
                
            
            -   예) A세션이 임시 상태에 있는 데이터 만들었다. 해당 데이터를 B세션이 데이터를 변경하고 저장했지만, A세션이 롤백하면 데이터 사라짐.  
                
    
    -   트랜잭션 - DB 예제2 - 자동 커밋, 수동 커밋  
        -   트랙잭션  
            
            -   자동 커밋  
                
                -   사용법![](https://api.transno.com/v3/document_image/ff7b1bfd-49d4-41ff-81b3-279538772a76-10826299.jpg)  
                    
                
                -   자동 커밋으로 설정하면 각각의 쿼리 실행 직후에 자동으로 커밋을 호출한다  
                    
                
                -   장점  
                    -   커밋이나 롤백을 직접 호출하지 않아도 되는 편리함  
                        
                
                -   단점  
                    -   우리가 원하는 트랜잭션 기능을 제대로 사용할 수 없다.  
                        
            
            -   수동 커밋  
                
                -   사용법![](https://api.transno.com/v3/document_image/6f67cef4-3f06-4cc9-9d50-70470492bddd-10826299.jpg)  
                    
                
                -   쿼리를 한번에 실행 후, 커밋과 롤백을 직접 설정함.  
                    
                
                -   장점  
                    -   트랙젹션 기능 사용  
                        
                
                -   단점  
                    -   커밋/롤백을 일일히 쳐줘야함.  
                        
    
    -   트랜잭션 - DB 예제 3/4 - 트랜잭션 실습/계좌이체  
        
        -   신규 데이터 추가 - 수동 커밋 전(세션1)  
            
            -   1) 두개의 다른 세션이 같은 테이블 값을 가지고 있게 만들기(기본설정)![](https://api.transno.com/v3/document_image/8390eb20-6d99-4de0-83e6-3f2e6c32896c-10826299.jpg)  
                
            
            -   2) 세션1에서는 입력한데이터가 보이지만, 세션2에서는 입력한 데이터가 보이지 않는 것을 확인![](https://api.transno.com/v3/document_image/7b40263d-fc57-4e70-add5-9b2372c7a680-10826299.jpg)![](https://api.transno.com/v3/document_image/5be77d4e-cab5-47a1-8002-578195677f06-10826299.jpg)  
                
            
            -   IF) 커밋 후(세션1) => 커밋 이후에는 모든 세션에서 데이터를 조회할 수 있음.![](https://api.transno.com/v3/document_image/689f9122-fe82-41f0-9644-8e43b8b68ee6-10826299.jpg)![](https://api.transno.com/v3/document_image/3e691920-dfe5-4264-a57a-4ce86c48ae78-10826299.jpg)  
                
            
            -   IF) 롤백 후 (세션1) => 롤백으로 데이터가 DB에 반영되지 않은 것을 확인할 수 있다.![](https://api.transno.com/v3/document_image/8b65f0e4-f41d-4f25-9c98-0abbe643a2fe-10826299.jpg)![](https://api.transno.com/v3/document_image/63666f7b-05b6-45e6-9d8e-6f6889a1dbff-10826299.jpg)  
                
        
        -   커밋 문제 상황  
            
            -   다음과 같이 SQL 오류가 난다면?![](https://api.transno.com/v3/document_image/6b02762a-adbf-45b6-a75d-66fb21fd093c-10826299.jpg)  
                
            
            -   어떤 문제가 생겨?  
                
                -   memberA 의 돈은 2000원 줄어들었지만, memberB 의 돈은 2000원 증가하지 않았다는 점이다. 결과적으로 계좌이체는 실패하고 memberA 의 돈만 2000원 줄어든 상황.  
                    
                
                -   이때 강제로 커밋하면, 문제상황 유지  
                    
                
                -   롤백하면 원래 데이터로 복구  
                    
    
    -   DB 락 - 개념 이해  
        -   세션이 트랜잭션을 시작하고 데이터를 수정하는 동안에는 커밋이나 롤백 전까지 다른 세션에서 해당 데이터를 수정할 수 없게 막아야 한다.  
            -   why? 트랜잭션의 원자성이 깨짐.  
                
    
    -   DB 락 - 변경  
        
        -   그림으로 DB락 변경 메커니즘  
            
            -   1) 기본 데이터를 입력하자.![](https://api.transno.com/v3/document_image/5416529b-7085-4fe1-88af-0eb8dd06202b-10826299.jpg)![](https://api.transno.com/v3/document_image/6055bba8-e20f-41af-a173-c4ebe4d6ed6b-10826299.jpg)  
                
            
            -   2) 세션1은 트랜잭션을 시작한다. 락이 남아 있으므로 세션1은 락을 획득한다.![](https://api.transno.com/v3/document_image/99e1a112-5ce3-4839-8456-2345e43594b6-10826299.jpg)![](https://api.transno.com/v3/document_image/ab0a1cd4-307c-4f15-be7e-24f008e576b5-10826299.jpg)  
                
            
            -   세션2는 트랜잭션을 시작한다. 하지만 락이 없으므로 락이 돌아올 때 까지 대기한다.![](https://api.transno.com/v3/document_image/8c21366d-24f2-47f4-8664-2178c675cc79-10826299.jpg)![](https://api.transno.com/v3/document_image/be20611e-c1ab-4b44-a16a-fd21f074f061-10826299.jpg)  
                락 대기 시간을 넘어가면 락 타임아웃 오류가 발생한다.  
                락 대기 시간은 설정할 수 있다.
            
            -   세션1이 커밋을 한다. 커밋으로 트랜잭션이 종료되었으므로 락도 반납한다.![](https://api.transno.com/v3/document_image/6e15b9d3-4763-4928-a541-326dabdd6c4b-10826299.jpg)![](https://api.transno.com/v3/document_image/6a4588ff-937c-4990-9cfa-985f0572d412-10826299.jpg)  
                
            
            -   타임 아웃전, 세션2가 락을 확득한다. update sql을 수행한다.![](https://api.transno.com/v3/document_image/fc91d1eb-ca8d-4c63-894b-cba791172e5c-10826299.jpg)![](https://api.transno.com/v3/document_image/d1568e3e-412e-4550-bb06-fb086fe3d631-10826299.jpg)  
                
            
            -   세션2는 커밋을 수행하고 트랜잭션이 종료되었으므로 락을 반납한다.![](https://api.transno.com/v3/document_image/64f9c1b8-a4ca-41d0-8446-012c071ba5ab-10826299.jpg)![](https://api.transno.com/v3/document_image/6ac9c389-0355-475c-a570-5615d45219bc-10826299.jpg)  
                
        
        -   타임 아웃 오류는?  
            -   타임아웃 오류 로그![](https://api.transno.com/v3/document_image/a61eb6b1-277d-43af-bccf-7e1eae8245aa-10826299.jpg)  
                
    
    -   DB 락 - 조회  
        
        -   일반적인 조회는 락을 사용하지 않는다. BUT 언제 사용하는가?  
            -   트랜잭션 종료 시점까지 해당 데이터를 다른 곳에서 변경하지 못하도록 강제로 막아야 할 때 사용한다. (ex 은행 마감시간)  
                
        
        -   사용법은?  
            -   select for update 구문을 사용![](https://api.transno.com/v3/document_image/b986e014-597c-4068-9fc7-6b0f0b85d4d8-10826299.jpg)  
                
        
        -   매커니즘은 락과 동일함.  
            
    
    -   트랜잭션 - 적용1  
        
        -   트랜잭션 없이 비즈니스 로직 구성 => 이해만![](https://api.transno.com/v3/document_image/9447431b-9f0e-4868-a7c6-36c8fe37b07c-10826299.jpg)  
            계좌이체 상황  
            MemberRepository 안에는 update, findById를 SQL로 만들어 놓은 구현체가 있음.  
            비즈니스 로직은 오직 자바 코드만.
        
        -   테스트 코드는 직접 코드로 보기  
            
    
    -   트랜잭션 - 적용2  
        
        -   트랜잭션을 어떤 계층에 걸어야 할까?  
            
            -   그림으로 이해하기![](https://api.transno.com/v3/document_image/a045f51b-8aba-46bf-99d4-fe96f0138bbd-10826299.jpg)  
                
            
            -   트랜잭션은 비즈니스 로직이 있는 서비스 계층에서 시작해야 한다.  
                -   why? 비즈니스 로직으로 인해 문제가 되는 부분을 함께 롤백해야 하기 때문이다.  
                    
            
            -   DB 트랜잭션을 사용하려면 트랜잭션을 사용하는 동안 같은 커넥션을 유지해야함.  
                -   HOW? 파라미터로 커넥션을 전달하여 사용하면 되지!!!  
                    
        
        -   코드로 보기 => 직접가서 보기  
            
            -   MemberRepositoryV2  
                
                -   새롭게 저장하는 함수를 만들어야함  
                    -   why? 그전 함수는 con이 유지되지 않기 때문  
                        
                
                -   1) con = getConnection() 코드 없애기  
                    
                
                -   2) finally에서 커넥션 종료하지 말고, 서비스 로직에서 종료하기  
                    
            
            -   MemberServiceV2  
                
                -   1) Connection con = dataSource.getConnection() 으로 하나의 커넥션 유지하기  
                    
                
                -   2) con.setAutoCommit(false) 으로 트랜잭션 시작  
                    
                
                -   3) bizLogic(con, fromId, toId, money) 으로 비즈니스 로직 실행  
                    
                
                -   4) 성공시 커밋, 실패시 롤백  
                    
                
                -   5) release(con) 으로 기본 값인 자동 커밋 모드로 변경하는 것 + 커넥션에 반납하기  
                    
        
        -   테스트 코드 위와 동일  
            
    
    -   중요 내용  
        
        -   트랜잭션을 왜 쓰는거지?  
            
        
        -   트랜잭션 ACID  
            
        
        -   세션  
            
        
        -   DB락  
            
        
        -   트랜잭션을 코드로 적용할때 고려해야 될점 2가지  
            
-   스프링과 문제 해결 - 트랜잭션  
    
    -   문제점들  
        
        -   애플리케이션 구조  
            
            -   그림![](https://api.transno.com/v3/document_image/af5d47be-5ee1-4ea0-ba36-e93e17c9906c-10826299.jpg)  
                
            
            -   프레젠테이션 계층  
                
                -   UI와 관련된 처리 담당  
                    
                
                -   웹 요청과 응답  
                    
                
                -   사용자 요청을 검증  
                    
                
                -   주 사용 기술: 서블릿과 HTTP 같은 웹 기술, 스프링 MVC  
                    
            
            -   서비스 계층  
                
                -   비즈니스 로직을 담당  
                    
                
                -   주 사용 기술: 가급적 특정 기술에 의존하지 않고, 순수 자바 코드로 작성  
                    
            
            -   데이터 접근 계층  
                
                -   계층실제 데이터베이스에 접근하는 코드  
                    
                
                -   주 사용 기술: JDBC, JPA, File, Redis, Mongo ...  
                    
        
        -   애플리케이션의 3가지 문제점  
            
            -   트랜잭션 문제  
                
                -   1) JDBC 구현 기술이 서비스 계층에 누수되는 문제  
                    -   서비스 계층은 특정 기술에 종속 X  
                        
                
                -   2) 트랜잭션 동기화 문제  
                    -   같은 트랜잭션을 유지하기 위해 커넥션을 파라미터로 넘겨야 한다. 그래서 똑같은 기능도 트랜잭션용 기능과 트랜잭션을 유지하지 않아도 되는기능으로 분리했음.  
                        
                
                -   3) 트랜잭션 적용 반복 문제  
                    -   try, catch, finally...  
                        
            
            -   예외 누수 문제  
                -   SQLException 은 JDBC 전용 기술이지만, 서비스 계층으로 전파된다.  
                    
            
            -   JDBC 반복 문제  
                -   try , catch , finally ...  
                    
        
        -   용어정리  
            
            -   누수되어 있다  
                -   섞여있다고 생각하자  
                    
            
            -   JDBC 기술에 의존하는 것들  
                -   javax.sql ~ 로 구성되어 있는 것들  
                    
                    -   javax.sql.DataSource  
                        
                    
                    -   java.sql.Connection  
                        
                    
                    -   java.sql.SQLException  
                        
            
            -   동기화  
                -   일치화하는 것  
                    
    
    -   트랜잭션 추상화  
        
        -   다른 데이터 접근 기술로 변경하면, 서비스 계층의 트랜잭션 관련 코드도 모두 함께 수정해야 한다. => 추상화하자  
            
        
        -   트랜잭션 추상화  
            
            -   인터페이스 => 스프링이 구현체 다 구현해놓음![](https://api.transno.com/v3/document_image/a1d6ca3e-8ea9-4d49-8196-309e8f712b2a-10826299.jpg)  
                
            
            -   의존관계![](https://api.transno.com/v3/document_image/a442f95b-54bc-42db-9778-5fb377f93b6c-10826299.jpg)  
                
    
    -   트랜잭션 동기화  
        -   트랜잭션 매니저  
            
            -   정의  
                -   쓰레드 로컬을 사용하기 때문에 멀티쓰레드 상황에 안전하게 커넥션을 동기화 할수 있다. 즉, 이전처럼 파라미터로 커넥션을 전달하지 않아도 된다.  
                    
            
            -   역할  
                
                -   트랜잭션 추상화  
                    
                
                -   리소스 동기화  
                    -   트랜잭션을 유지하려면 트랜잭션의 시작부터 끝까지 같은 데이터베이스 커넥션을 유지함.  
                        
            
            -   그림  
                
                -   기존 커넥션 세션![](https://api.transno.com/v3/document_image/af6ea54e-58b6-4887-bb3b-05080ee63c07-10826299.jpg)  
                    
                
                -   트랜잭션 매니저와 트랜잭션 동기화 매니저![](https://api.transno.com/v3/document_image/7e9c95e9-1a34-436b-83fa-c3c4432eea91-10826299.jpg)  
                    
                
                -   동작방식  
                    
                    -   1. 트랜잭션을 시작하려면 커넥션이 필요하다. 트랜잭션 매니저는 데이터소스를 통해 커넥션을 만들고 트랜잭션을 시작한다.  
                        
                    
                    -   2. 트랜잭션 매니저는 트랜잭션이 시작된 커넥션을 트랜잭션 동기화 매니저에 보관한다.  
                        
                    
                    -   3. 리포지토리는 트랜잭션 동기화 매니저에 보관된 커넥션을 꺼내서 사용한다. 따라서 파라미터로 커넥션을 전달하지 않아도 된다.  
                        
                    
                    -   4. 트랜잭션이 종료되면 트랜잭션 매니저는 트랜잭션 동기화 매니저에 보관된 커넥션을 통해 트랜잭션을 종료하고, 커넥션도 닫는다.  
                        
    
    -   트랜잭션 매니저1  
        -   코드는 직접 가서 보기  
            
            -   MemberRepositoryV3  
                -   트랜잭션 매니저 코드 설명  
                    
                    -   DataSourceUtils.getConnection()  
                        
                        -   트랜잭션 동기화 매니저가 관리하는 커넥션이 있으면 해당 커넥션을 반환한다.  
                            
                        
                        -   트랜잭션 동기화 매니저가 관리하는 커넥션이 없는 경우(ex 자동커밋) 새로운 커넥션을 생성해서 반환한다.  
                            
                    
                    -   DataSourceUtils.releaseConnection()  
                        
                        -   트랜잭션을 사용하기 위해 동기화된 커넥션은 커넥션을 닫지 않고 그대로 유지해준다.  
                            
                        
                        -   트랜잭션 동기화 매니저가 관리하는 커넥션이 없는 경우 해당 커넥션을 닫는다.  
                            
            
            -   MemberServiceV3_1  
                -   트랜잭션 매니저 코드 설명  
                    -   private final PlatformTransactionManager transactionManager  
                        -   트랜잭션 매니저를 주입 받는다.  
                            
    
    -   트랜잭션 매니저2  
        -   트랜잭션 매니저 메커니즘 정리  
            
            -   클라이언트의 요청으로 서비스 로직을 실행전  
                
                -   트랜잭션 시작![](https://api.transno.com/v3/document_image/b026ae7c-380b-4499-9d89-0844214b8a1f-10826299.jpg)  
                    
                
                -   1. 서비스 계층에서 transactionManager.getTransaction() 을 호출해서 트랜잭션을 시작한다.  
                    
                
                -   2. 트랜잭션을 시작하려면 먼저 데이터베이스 커넥션이 필요하다. 트랜잭션 매니저는 내부에서 데이터소스를 사용해서 커넥션을 생성한다.  
                    
                
                -   3. 커넥션을 수동 커밋 모드로 변경해서 실제 데이터베이스 트랜잭션을 시작한다.  
                    
                
                -   4. 트랜잭션 동기화 매니저에 커넥션을 보관한다.  
                    
                
                -   5. 트랜잭션 동기화 매니저는 쓰레드 로컬에 커넥션을 보관한다. 따라서 멀티 쓰레드 환경에 안전하게 커넥션을 보관할 수 있다.  
                    
            
            -   로직 실행  
                
                -   로직 실행![](https://api.transno.com/v3/document_image/ee6708d7-d59e-46e0-8bbe-37fc5a81ee7d-10826299.jpg)  
                    
                
                -   6. 서비스는 비즈니스 로직을 실행하면서 리포지토리의 메서드들을 호출한다. 이때 커넥션을 파라미터로 전달하지 않는다.  
                    
                
                -   7. 리포지토리 메서드들은 트랜잭션이 시작된 커넥션이 필요하다. 리포지토리는 DataSourceUtils.getConnection() 을 사용해서 트랜잭션 동기화 매니저에 보관된 커넥션을 꺼내서 사용한다. 이 과정을 통해서 자연스럽게 같은 커넥션을 사용하고, 트랜잭션도 유지된다.  
                    
                
                -   8. 획득한 커넥션을 사용해서 SQL을 데이터베이스에 전달해서 실행한다.  
                    
            
            -   트랜잭션 종료  
                
                -   트랜잭션 종료![](https://api.transno.com/v3/document_image/c3c92f43-b895-4f1f-9a78-f190a906707f-10826299.jpg)  
                    
                
                -   9. 비즈니스 로직이 끝나고 트랜잭션을 종료한다. 트랜잭션은 커밋하거나 롤백하면 종료된다.  
                    
                
                -   10. 트랜잭션을 종료하려면 동기화된 커넥션이 필요하다. 트랜잭션 동기화 매니저를 통해 동기화된 커넥션을 획득한다.  
                    
                
                -   11. 획득한 커넥션을 통해 데이터베이스에 트랜잭션을 커밋하거나 롤백한다.  
                    
                
                -   12. 전체 리소스를 정리한다.  
                    
                    -   트랜잭션 동기화 매니저를 정리한다.  
                        
                    
                    -   쓰레드 로컬은 사용후 꼭 정리해야 한다. con.setAutoCommit(true) 로 되돌린다. 커넥션 풀을 고려해야 한다.  
                        
                    
                    -   con.close() 를 호출해셔 커넥션을 종료한다. 커넥션 풀을 사용하는 경우 con.close() 를 호출하면 커넥션 풀에 반환된다.  
                        
    
    -   트랜잭션 템플릿  
        
        -   다른 서비스에서 트랜잭션을 시작하려면 try , catch , finally 를 포함한 성공시 커밋, 실패시 롤백 코드가 반복되는 문제점  
            -   코드로 보기![](https://api.transno.com/v3/document_image/1aa88f7d-10a6-4db1-bace-08490be8c204-10826299.jpg)  
                
        
        -   해결방법  
            -   템플릿 콜백 패턴을 적용하려면 템플릿을 제공하는 클래스를 작성![](https://api.transno.com/v3/document_image/1d9e95e1-4656-4f16-b732-0301efa4d332-10826299.jpg)  
                
    
    -   트랜잭션 AOP 이해  
        
        -   이제는 서비스 계층에 순수한 비즈니스 로직만 남긴다는 목표로 코드로 알아본 후 애노테이션으로 간단하게 해보자  
            
        
        -   그림으로 이해하기  
            
            -   프록시 도입전  
                
                -   그림![](https://api.transno.com/v3/document_image/78856ad7-8d42-44b8-9553-2f4c6b25776d-10826299.jpg)  
                    
                
                -   코드 예시![](https://api.transno.com/v3/document_image/d1d94a70-53a9-4a6e-9a0a-19125b6c512b-10826299.jpg)  
                    
            
            -   프록시 도입후  
                
                -   그림![](https://api.transno.com/v3/document_image/b6c58061-05ff-4e26-9c24-5a2f3398e3cc-10826299.jpg)  
                    
                
                -   프록시 코드 예시![](https://api.transno.com/v3/document_image/9033112e-9fc4-4867-bbfb-4fb5ec858d3c-10826299.jpg)  
                    
                
                -   서비스 코드 예시![](https://api.transno.com/v3/document_image/7a9277af-0550-42e9-ae04-9fbbb1e56bf1-10826299.jpg)  
                    
        
        -   스프링이 제공하는 트랜잭션 AOP  
            
            -   스프링부트를 사용하면 트랜잭션 AOP를 처리하기 위해 필요한 스프링 빈들도 자동으로 등록  
                
            
            -   @ Transactional 을 사용하면 스프링이 AOP를 사용해서 트랜잭션을 편리하게 처리해준다 정도로 이해  
                
    
    -   트랜잭션 AOP 적용  
        -   코드로 직접보기  
            -   MemberServiceV3_3  
                
    
    -   트랜잭션 AOP 정리  
        -   트랜잭션 AOP 적용 전체 흐름  
            
            -   그림![](https://api.transno.com/v3/document_image/fb7c0759-4378-4673-abfc-1d6f1ed8ad7e-10826299.jpg)  
                
            
            -   선언적 트랜잭션 관리 => 가장 선호  
                -   @ Transactional 애노테이션 하나만 선언해서 매우 편리하게 트랜잭션을 적용하는 것을 선언적 트랜잭션 관리  
                    
            
            -   프로그래밍 방식의 트랜잭션 관리  
                -   트랜잭션 매니저 또는 트랜잭션 템플릿 등을 사용해서 트랜잭션 관련 코드를 직접 작성하는 것을 프로그래밍 방식의 트랜잭션 관리  
                    
    
    -   스프링 부트의 자동 리소스 등록  
        
        -   test 코드에서 직접 스프링 빈으로 등록해서 사용하는거 불편함.. 그래서 데이터소스나 트랜잭션 매니저를 스프링부트로 자동등록 해보자  
            
        
        -   데이터소스 - 자동 등록  
            
            -   자동으로 등록되는 스프링 빈 이름: dataSource  
                
            
            -   기본으로 생성하는 데이터소스는 커넥션풀을 제공하는 HikariDataSource  
                
            
            -   속성을 사용해서 DataSource를 생성하는 법![](https://api.transno.com/v3/document_image/31d6d30b-6419-4c61-9f81-77c3ef4091de-10826299.jpg)  
                
        
        -   트랜잭션 매니저 - 자동 등록  
            
            -   자동으로 등록되는 스프링 빈 이름: transactionManager  
                
            
            -   어떤 트랜잭션 매니저를 선택할지는 현재 등록된 라이브러리를 보고 판단함  
                
                -   JDBC 기술 -> DataSourceTransactionManager  
                    
                
                -   JPA 기술 -> JpaTransactionManager  
                    
                
                -   JDBC + JPA 기술 -> JpaTransactionManager  
                    
    
    -   중요내용  
        -   문제점  
            
            -   트랜잭션 동기화 문제  
                -   트랜잭션 매니저에서 생성한 커넥션을 트랜잭션 동기화 매니저에 보관한다  
                    
            
            -   트랜잭션 반복 문제  
                -   콜백패턴으로 해결 이후 @ Transactional로 더 간단히 함  
                    
            
            -   예외 누수 문제  
                -   체크예외를 런타임 예외로 변환함  
                    
-   자바 예외 이해  
    
    -   예외 계층  
        -   예외 계층  
            
            -   그림으로 보기![](https://api.transno.com/v3/document_image/328df83a-e570-44df-9c2d-403a2dab4e05-10826299.jpg)  
                
            
            -   Error  
                -   복구 불가능한 시스템 예외이다. 신경쓰지 말기  
                    
            
            -   Exception  
                
                -   사용할 수 있는 실질적인 최상위 예외  
                    
                
                -   그 하위 예외는 모두 컴파일러가 체크하는 체크 예외이다.  
                    
                
                -   예외인 경우  
                    
                    -   RuntimeException  
                        
                    
                    -   illegalArgumentException  
                        
    
    -   예외 기본 규칙  
        
        -   예외처리 두가지 규칙  
            
            -   1) 예외는 잡아서 처리하거나 던져야 한다.  
                
                -   잡는 경우![](https://api.transno.com/v3/document_image/db99f56d-d19d-46e4-9c95-4c521dc350a8-10826299.jpg)  
                    
                
                -   던지는 경우![](https://api.transno.com/v3/document_image/9e37e222-9d54-4458-a9dd-56895c5e0108-10826299.jpg)  
                    
            
            -   2. 예외를 잡거나 던질 때 지정한 예외뿐만 아니라 그 예외의 자식들도 함께 처리된다.  
                
        
        -   계속 던지면 어떻게 될까?  
            
            -   자바 main() 쓰레드의 경우, 예외 로그를 출력하면서 시스템이 종료된다.  
                
            
            -   애플리케이션의 경우, WAS가 해당 예외를 받아서 처리하는데, 주로 사용자에게 개발자가 지정한, 오류 페이지를 보여준다.  
                
    
    -   체크 예외 기본 이해  
        
        -   코드는 직접 보기 => CheckedTest  
            
        
        -   예외를 잡아서 처리하는 매커니즘은?  
            
            -   그림![](https://api.transno.com/v3/document_image/43542aa9-2c4b-429d-9dbc-71fc0463abf0-10826299.jpg)  
                
            
            -   test![](https://api.transno.com/v3/document_image/d376984e-f6e2-41b4-addd-b244aa74a56d-10826299.jpg)  
                
            
            -   Service => 이 부분에서 예외처리함![](https://api.transno.com/v3/document_image/e361ce5f-252b-4d80-8db2-6b171d258fc2-10826299.jpg)  
                
            
            -   Repository => 예외 발생부분![](https://api.transno.com/v3/document_image/e4887d30-46d5-42d6-94f8-af5f3ffd5ce2-10826299.jpg)  
                
        
        -   예외를 계속 던져서 처리하는 매커니즘은?  
            
            -   그림![](https://api.transno.com/v3/document_image/f6c95e1f-8c69-4912-8fea-4b908ffa42fd-10826299.jpg)  
                
            
            -   Test => 예외를 던져서 처리![](https://api.transno.com/v3/document_image/873e3664-a127-4c80-8eb1-0bbd82b09b04-10826299.jpg)  
                
            
            -   Service![](https://api.transno.com/v3/document_image/2f6a9d13-9512-459f-b4d4-4da68a5ffdd0-10826299.jpg)  
                
            
            -   Repository => 예외 발생부분![](https://api.transno.com/v3/document_image/e4887d30-46d5-42d6-94f8-af5f3ffd5ce2-10826299.jpg)  
                
        
        -   IF 체크 예외인데, 예외를 던지지 않는다면?  
            -   throws 를 지정하지 않으면 컴파일 오류가 발생.![](https://api.transno.com/v3/document_image/cefff618-5952-4a50-8493-deb4f19543f9-10826299.jpg)  
                
        
        -   체크 예외의 장단점  
            
            -   장점  
                -   예외를 누락하지 않도록 컴파일러를 통해 문제를 잡아주는 훌륭한 안전 장치  
                    
            
            -   단점  
                
                -   번거로운 일  
                    
                
                -   의존관계에 따른 예외처리 수정  
                    
    
    -   언체크 예외 기본 이해  
        
        -   체크 예외 VS 언체크 예외  
            
            -   체크 예외: 예외를 잡아서 처리하지 않으면 항상 throws 에 던지는 예외를 선언해야 한다.  
                
            
            -   언체크 예외: 예외를 잡아서 처리하지 않아도 throws 를 생략할 수 있다.  
                
        
        -   이전 체크 예외에서 throws 부분 없어지고 런타임 에러로 바뀜  
            
        
        -   언체크 예외의 장단점  
            
            -   장점 : 언체크 예외를 무시할 수 있다  
                
            
            -   단점 : 개발자가 실수로 예외를 누락할 수 있다.  
                
    
    -   체크 예외 활용  
        
        -   언제 체크 예외를 사용하고 언제 언체크(런타임) 예외를 사용하면 좋을까?  
            
            -   기본적으로 언체크(런타임) 예외를 사용  
                
            
            -   체크 예외는 비즈니스 로직상 의도적으로 던지는 예외에만 사용 ex) 계좌 이체 실패 예외  
                
        
        -   체크 예외의 문제점  
            
            -   그림![](https://api.transno.com/v3/document_image/424944af-2a6d-4782-a682-50ab30aa0991-10826299.jpg)  
                
            
            -   1. 복구 불가능한 예외  
                
                -   대부분의 서비스나 컨트롤러는 이런 문제들을 해결할 수 없어서 일관성 있게 공통적으로 처리해야함.  
                    
                
                -   오류 로그를 남기고 개발자가 해당 오류를 빠르게 인지하는 것이 필요하다.  
                    
            
            -   2. 의존 관계에 대한 문제  
                -   JDBC 기술이 아닌 다른 기술로 변경한다면, 오류 또한 다 바꿔줘야함 OMG.![](https://api.transno.com/v3/document_image/47468030-1e49-4de7-900f-5fdf5372f355-10826299.jpg)  
                    
        
        -   throws Exception  
            -   최상위 타입인 Exception 을 던지게 되면 다른 체크 예외를 체크할 수 있는 기능이 무효화 => 안티패턴  
                
    
    -   언체크 예외 활용  
        
        -   런타임 예외 사용하기  
            -   그림![](https://api.transno.com/v3/document_image/cbc896f2-4402-487b-9d8d-5409eb5671ae-10826299.jpg)  
                
        
        -   런타임 예외로 바꾸는 방법  
            
            -   1) 체크 예외를 런타임 예외로 변환해서 사용  
                -   SQLException 을 런타임 예외인 RuntimeSQLException 으로 변환  
                    
            
            -   2) 체크 예외 대신 같은 기능이 있는 런타임 예외로 사용  
                -   ConnectException 대신에 RuntimeConnectException 을 사용  
                    
        
        -   런타임 예외는 문서화  
            -   예) method() throws DataAccessException 와 같이 문서화 + 코드에도 명시![](https://api.transno.com/v3/document_image/0157e5c5-c887-416c-9fb2-bce22b464cf3-10826299.jpg)  
                
    
    -   예외 포함과 스택 트레이스  
        
        -   예외를 전환할 때는 꼭! 기존 예외를 포함  
            
        
        -   코드의 어느 부분 말하는 거지?  
            -   예외 발생 코드![](https://api.transno.com/v3/document_image/6ea23e5d-891b-4dfb-a0d2-4ab1b53fab08-10826299.jpg)  
                
        
        -   기존 예외 포함하는 경우  
            
            -   코드![](https://api.transno.com/v3/document_image/34091b39-ce80-499f-b74d-611a9f5596c6-10826299.jpg)  
                
            
            -   결과![](https://api.transno.com/v3/document_image/0e4510e7-82be-4a0f-8d4d-cadbeb1df867-10826299.jpg)  
                
        
        -   기존 예외 포함하지 않는 경우  
            
            -   코드![](https://api.transno.com/v3/document_image/a08f93a2-ed5b-48c3-94e8-433f3bdae2f7-10826299.jpg)  
                
            
            -   결과![](https://api.transno.com/v3/document_image/238ecb21-3b5a-4e4b-8b64-17375e59554a-10826299.jpg)  
                
    
    -   중요내용  
        
        -   예외가 발생했을떄 해야되는 것  
            
            -   던지기  
                
            
            -   처리하기  
                
        
        -   런타임 예외가 기본 설정으로 하는 2가지 이유  
            
            -   복구 불가능한 오류  
                
            
            -   의존관계에 대한 문제  
                
        
        -   런타임 예외로 전환하는 2가지 방법  
            
            -   클래스를 만들어 체크예외를 런타임 예외로 전환하기  
                
            
            -   같은 기능이 있는 런타임 예외로 교체하기  
                
        
        -   런타임 예외 전환시 꼭 해야할 2가지  
            
            -   문서화  
                
            
            -   오류 내용 포함하기  
                
-   스프링과 문제 해결  
    
    -   체크 예외와 인터페이스  
        
        -   서비스가 처리할 수 없는 SQLException 에 대한 의존을 제거하려면 어떻게 해야할까?  
            -   런타임 예외로 전환해서 서비스 계층에 던지자  
                
        
        -   인터페이스 도입시 문제점  
            
            -   특정 기술에 종속되는 인터페이스  
                
            
            -   인터페이스로 보는 코드![](https://api.transno.com/v3/document_image/7f665a46-f8e9-43d5-a9ff-ad188f8da2a4-10826299.jpg)  
                
            
            -   구현 클래스로 보는 코드![](https://api.transno.com/v3/document_image/73f82ef3-4e79-4597-82a3-489138db613c-10826299.jpg)  
                
    
    -   런타임 예외 적용  
        
        -   런타임 예외 선언  
            
            -   코드![](https://api.transno.com/v3/document_image/79b9b123-b864-43e4-a134-5349dbb8292a-10826299.jpg)  
                기존에 체크예외를 런타임예외로 바꾸기위해 선언함.  
                ​메세지 가져오기/ 원인 에러 가져오기로 함수 기본 정의
            
            -   코드로 보기  
                
                -   SQLException 이라는 체크 예외를 MyDbException 이라는 런타임 예외로 변환  
                    
                
                -   주의할점 -> 기존 예외를 무시하고 작성하면 절대 안된다!![](https://api.transno.com/v3/document_image/0c16e1fc-1278-49fc-a416-47287e9ab280-10826299.jpg)  
                    
        
        -   덕분에 JDBC에서 다른 구현 기술로 변경하더라도 서비스 계층의 코드를 변경하지 않고 유지할 수있음.  
            
    
    -   데이터 접근 예외 직접 만들기  
        
        -   회원 가입시 DB에 이미 같은 ID가 있으면 ID 뒤에 숫자를 붙여서 새로운 ID를 만들어야 하는 문제  
            
        
        -   오류 매커니즘은?  
            
            -   데이터 베이스 오류 그림![](https://api.transno.com/v3/document_image/f4e9e467-6ac6-4b95-843b-fbadde36afa1-10826299.jpg)  
                
            
            -   문제점  
                
                -   SQL ErrorCode는 각각의 데이터베이스마다 다르다. 결과적으로 데이터베이스가 변경될 때 마다. ErrorCode도 모두 변경해야 한다 ex) H2: 23505  
                    
                
                -   SQLException 을 서비스 계층에 던지고 서비스 계층은 이 예외의 오류 코드를 확인해서 키 중복 오류( 23505 )인 경우 새로운 ID를 만들어서 다시 저장함.  
                    -   서비스 계층이 SQLException 이라는 JDBC 기술에 의존하게 되면서, 지금까지 우리가 고민했던 서비스 계층의 순수성이 무너진다.  
                        
        
        -   해결방안은?  
            
            -   오류 코드 추상화하기 => 스프링에서 제공해줌  
                
            
            -   SQLException => MyDuplicateKeyException 으로 고치기  
                
        
        -   코드로 보기  
            -   ExTranslatorV1Test  
                
                -   Repository![](https://api.transno.com/v3/document_image/9f70c830-486e-4fb6-9c59-fdeb7f585140-10826299.jpg)  
                    
                    -   SQLException이 터졌을때, 런타임 예외로 전환하는 역할  
                        
                    
                    -   특정 에러코드마다 다른 런타임 예외로 전환함 ex) 23505 -> MyDuplicateKeyException  
                        
                
                -   Service![](https://api.transno.com/v3/document_image/05ad6a36-8e0d-41d0-a813-8b31b5054861-10826299.jpg)  
                    
                    -   Repository에서 던진 예외를 잡아서 처리해줌  
                        
                    
                    -   catch를 이용해 예외를 개별적으로 처리했음.  
                        
    
    -   스프링 예외 추상화 이해  
        
        -   스프링에서 제공하는 런타임 에러 예외의 추상화  
            -   그림![](https://api.transno.com/v3/document_image/083322e5-de02-4618-9760-ef690d4fe1bc-10826299.jpg)  
                
                -   Transient  
                    -   하위 예외는 동일한 SQL을 다시 시도했을 때성공할 가능성이 있다.  
                        
                
                -   NonTransient  
                    
                    -   같은 SQL을 그대로 반복해서 실행하면 실패한다.  
                        
                    
                    -   ex) SQL 문법 오류, 데이터베이스 제약조건 위배 ....  
                        
        
        -   스프링이 제공하는 예외 변환기  
            
            -   이전에는 H2: 23505와 같이 데이터베이스마다 다른 오류 코드를 갖고 있어 변경에 어려움을 겪었다. 그래서 이러한 에러 코드를 자동으로 변환해주는 기능을 스프링이 제공함.  
                
            
            -   코드로 보기![](https://api.transno.com/v3/document_image/484db0a0-86b7-4897-9ed5-96ffe2522086-10826299.jpg)  
                -   translate() 메서드  
                    -   첫번째 파라미터는 읽을 수 있는 설명이고, 두번째는 실행한 sql, 마지막은 발생된SQLException 을 전달  
                        
            
            -   어떻게 각각의 DB가 제공하는 SQL ErrorCode까지 고려해서 예외를 변환할 수 있을까?  
                -   스프링에서 에러코드 정리![](https://api.transno.com/v3/document_image/a33b2bf5-0f2b-4f6c-882a-9eb8a3cf492f-10826299.jpg)  
                    
    
    -   스프링 예외 추상화 적용  
        -   코드로 보기  
            -   MemberRepositoryV4_2  
                -   기존 코드에서 스프링 예외 변환기를 사용하도록 변경![](https://api.transno.com/v3/document_image/5994a854-25e8-4224-8c85-29c305a2e485-10826299.jpg)  
                    
    
    -   JDBC 반복 문제 해결 - JdbcTemplate  
        
        -   JDBC 반복 문제  
            
            -   커넥션 조회, 커넥션 동기화  
                
            
            -   PreparedStatement 생성 및 파라미터 바인딩  
                
            
            -   쿼리 실행  
                
            
            -   결과 바인딩  
                
            
            -   예외 발생시 스프링 예외 변환기 실행  
                
            
            -   리소스 종료  
                
        
        -   코드로 보기(템플릿 콜백 패턴)  
            -   MemberRepositoryV5  
                -   JdbcTemplate  
                    -   트랜잭션을 위한 커넥션 동기화는 물론이고, 예외 발생시 스프링 예외 변환기도 자동으로실행![](https://api.transno.com/v3/document_image/ccf7acb3-b060-43c8-b0b1-c9f717901a0d-10826299.jpg)![](https://api.transno.com/v3/document_image/a3049cbc-2413-43b5-9c67-8e823715d67a-10826299.jpg)  
                        
    
    -   중요 내용  
        -