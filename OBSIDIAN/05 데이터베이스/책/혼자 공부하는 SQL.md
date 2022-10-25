-   Chapter 01 데이터베이스와 SQL  
    
    -   데이터 베이스란?  
        -   데이터의 집합![](https://api.transno.com/v3/document_image/a521100e-a24e-4754-a7c2-432ad50c8fb4-10826299.jpg)  
            
    
    -   DBMS란?  
        -   데이터베이스를 운영하고 관리하는 소프트웨어로, 여러 명의 사용자가 공유하고 동시에 접근할 수 있게 함.  
            
    
    -   SQL이란?  
        -   DBMS에 데이터 구축, 관리하고 활용하기 위한 언어  
            
    
    -   [관계형 데이터베이스(Sql) vs 관계형 데이터베이스 (Nosql)의 차이는?](https://lazyer.tistory.com/27)  
        
-   Chapter 02 실전용 SQL 미리 맛보기  
    
    -   용어정리1  
        -   용어1![](https://api.transno.com/v3/document_image/2c03176b-c1f9-4281-bb0f-c6be91ebf344-10826299.jpg)  
            
    
    -   데이터 베이스 구축 절차  
        -   [구축절차](https://m.blog.naver.com/calb30095/222054766859)![](https://api.transno.com/v3/document_image/656f0f9a-2b79-4d6d-af23-493881ef7c98-10826299.jpg)  
            
            -   1) 설치![](https://api.transno.com/v3/document_image/65fa4a07-8a50-459f-877f-78dd31ec040b-10826299.jpg)  
                
            
            -   2) DB 만들기(=스카마 만들기) ![](https://api.transno.com/v3/document_image/6501f724-8b5e-41ef-aaef-5d089c80b85e-10826299.jpg)  
                
            
            -   3) 테이블 만들기![](https://api.transno.com/v3/document_image/cf793e17-1259-42f2-af0f-2663059c4973-10826299.jpg)![](https://api.transno.com/v3/document_image/3573550b-c9e0-4fa8-b2d2-cdab7b3a5487-10826299.jpg)![](https://api.transno.com/v3/document_image/daf86db4-6dd6-4023-92cf-7eee9bf7864b-10826299.jpg)  
                
            
            -   4) 테이블 채우기![](https://api.transno.com/v3/document_image/e5320c42-5f67-4ac2-9e6a-510c3c93508c-10826299.jpg)  
                
            
            -   5) 데이터 활용하기![](https://api.transno.com/v3/document_image/dcaa5d7c-5c21-4d73-88f3-7a26f8e537d9-10826299.jpg)![](https://api.transno.com/v3/document_image/0fcc5092-85b5-47e2-82bc-a79a69e25465-10826299.jpg)  
                
    
    -   용어정리2  
        -   용어2![](https://api.transno.com/v3/document_image/a0763988-beee-4d68-9642-ed99ff0a183c-10826299.jpg)  
            
    
    -   인덱스  
        
        -   인덱스란?  
            -   책에 '찾아보기'와 비슷한 개념  
                
        
        -   [[mysql] 인덱스 정리 및 팁 (tistory.com)](https://jojoldu.tistory.com/243)  
            
        
        -   인덱스 실습  
            
            -   이전 SQL -> Full Table Sacn![](https://api.transno.com/v3/document_image/d6d35d3f-a7e3-49c9-ae47-85a03ee98e88-10826299.jpg)  
                
            
            -   인덱스 활용 -> key lookup![](https://api.transno.com/v3/document_image/4d971e66-68d4-4987-b98e-e8d381ad7feb-10826299.jpg)  
                
    
    -   뷰  
        
        -   뷰란?  
            -   가상의 테이블로 실제 데이터를 가지고 있지 않으며, 진짜 테이블에 링크된 개념![](https://api.transno.com/v3/document_image/4507d610-3497-4a5a-b476-24dbd306fdc6-10826299.jpg)  
                
        
        -   뷰를 사용하는 이유는?  
            
            -   보안에 도움  
                -   why? 회원테이블의 정보를 전부 다 주는 것이 아니라 뷰를 통해서 선택적으로 만들기 때문에  
                    
            
            -   복잡한 sql을 간단한게 만들 수 있음  
                -   복잡하고 자주쓰는 sql문을 뷰로 만들면 되니깐!!  
                    
        
        -   뷰 작동방식  
            -   그림![](https://api.transno.com/v3/document_image/2bbad54f-7131-4371-adc8-994ceb559e36-10826299.jpg)  
                
        
        -   뷰 실습  
            
            -   테이블 생성하기  
                
                -   1) 스키마 선택하기  
                    
                
                -   2) 뷰 생성하기![](https://api.transno.com/v3/document_image/27373c9e-5182-4d75-bf48-58a7a662924e-10826299.jpg)  
                    
                
                -   3) 실행해보기![](https://api.transno.com/v3/document_image/5d6c223a-b722-4834-b229-0dad4a907da0-10826299.jpg)  
                    동일한 결과 나옴
            
            -   테이블 예시  
                
                -   생성![](https://api.transno.com/v3/document_image/1d4ac9a5-9304-4eea-ac93-b270e86b3969-10826299.jpg)  
                    
                
                -   수정![](https://api.transno.com/v3/document_image/5a32f3ea-e07f-4704-9843-f727573beaa9-10826299.jpg)  
                    
                
                -   삭제![](https://api.transno.com/v3/document_image/3bc33694-104d-4f9b-a256-d8167494385b-10826299.jpg)  
                    
                
                -   정보확인![](https://api.transno.com/v3/document_image/cf7c5c29-5406-4ce9-a23e-5888b53fa415-10826299.jpg)  
                    
                
                -   소스코드확인![](https://api.transno.com/v3/document_image/c6dbe6a1-078f-46a7-9e6e-a636a5b61c89-10826299.jpg)  
                    
            
            -   데이터 예시  
                
                -   입력![](https://api.transno.com/v3/document_image/e9c79b09-ef71-4f27-b401-684ba3a02a15-10826299.jpg)  
                    -   입력시 문제점  
                        
                        -   원래 테이블에 not null이 설정되어 있는 경우 반드시 입력해야된다.  
                            
                        
                        -   그래서 그 열을 포함하도록 재정의 하거나, 기본값을 지정해야함  
                            
                
                -   수정![](https://api.transno.com/v3/document_image/7b17dab9-fb6f-49c2-8a72-0826f1c40daa-10826299.jpg)  
                    
                
                -   삭제![](https://api.transno.com/v3/document_image/61d88c87-9971-4cd9-9fb1-11e1d8805257-10826299.jpg)  
                    
    
    -   스토어드 프로시저  
        
        -   스토어드 프로시저란?  
            -   mysql에서 제공하는 프로그래밍 기능  
                
        
        -   실습  
            
            -   1)함수 만들기![](https://api.transno.com/v3/document_image/10b510aa-4703-489b-baa1-fb985ef11962-10826299.jpg)  
                
            
            -   2)실행하기![](https://api.transno.com/v3/document_image/21173b31-36af-45b5-b958-cad551030c61-10826299.jpg)  
                
-   Chapter 03,04 SQL 기본 문법  
    
    -   문서 코딩테스트를 위한 Mysql 문법정리 참고  
        
    
    -   sql프로그래밍  
        
        -   구조  
            -   이미지![](https://api.transno.com/v3/document_image/60bc08b7-4108-4505-90b6-607d4e14d25f-10826299.jpg)  
                
        
        -   iF문  
            
            -   사용![](https://api.transno.com/v3/document_image/d034683c-10e4-4b2a-824c-56369dab36ae-10826299.jpg)  
                
            
            -   활용![](https://api.transno.com/v3/document_image/1677ffc2-6245-46ad-8fa1-85cabe7649d2-10826299.jpg)![](https://api.transno.com/v3/document_image/81c45f00-b6fe-4ce4-8e04-a635538ee6e3-10826299.jpg)  
                
        
        -   case문  
            
            -   구조![](https://api.transno.com/v3/document_image/c8cddecb-645d-42ba-96db-daba2e954542-10826299.jpg)  
                
            
            -   사용![](https://api.transno.com/v3/document_image/10f9d184-ffb7-4cee-9949-cfd1140d5067-10826299.jpg)![](https://api.transno.com/v3/document_image/9bad4b62-b4bd-444b-a160-8d3e42f64efa-10826299.jpg)  
                
            
            -   활용 -> select문에서 구간을 나눌때![](https://api.transno.com/v3/document_image/686c8394-8b43-4ca8-b9d2-b63fc52a9842-10826299.jpg)  
                
        
        -   while문  
            
            -   구조![](https://api.transno.com/v3/document_image/ae8dccbf-8bb0-4ea1-bc08-10b4fe1b96b0-10826299.jpg)  
                
            
            -   사용![](https://api.transno.com/v3/document_image/10c23d14-efe4-4bac-bf9e-38ebbafcfb66-10826299.jpg)![](https://api.transno.com/v3/document_image/1adfcd9b-5652-44c7-aed7-0fe4601496b4-10826299.jpg)  
                
        
        -   동적 sql문  
            -   when?  
                -   상황에 따라 내용 변경이 필요할 때  
                    
-   Chapter 05 테이블과 뷰  
    
    -   테이블 만들기  
        
        -   1) db생성![](https://api.transno.com/v3/document_image/a042f788-01e6-40c6-be8b-81d6bd39b76c-10826299.jpg)  
            
        
        -   2) 테이블 생성![](https://api.transno.com/v3/document_image/6977ba74-75b7-4956-8863-78fdf0bf510b-10826299.jpg)  
            테이블 생성할때 GUI로 만들 수 있진 연관관계는 직접 코드로 추가해줘야함
        
        -   3) 데이터 입력![](https://api.transno.com/v3/document_image/15d45592-a74e-4637-8550-2059509c60ea-10826299.jpg)  
            
    
    -   제약조건  
        
        -   테이블을 만들때, 데이터의 무결성을 지키기 위해 제한하는 조건  
            
        
        -   기본키(PK)  
            
            -   데이터를 구분할 수 있는 식별자로, 테이블 마다 1개만 가능  
                
            
            -   중복 X, null X  
                
            
            -   클러스터형 인덱스 생성  
                
        
        -   외래키(FK)  
            
            -   두 테이블 사이의 관계를 연결해주고, 다른 테이블의 기본키/고유키와 연결됌  
                
            
            -   기준 테이블의 열이 변경될 경우  
                -   오류발생 why? PK-FK 관계가 맺어지면 PK변경과 삭제 안됌.![](https://api.transno.com/v3/document_image/d9d92fd0-7f3b-45e0-a20b-b17da2383023-10826299.jpg)  
                    
            
            -   on update cascade/on delete cascade문 이용  
                
                -   코드![](https://api.transno.com/v3/document_image/abc52fe7-6869-4a15-878d-33fe94ec1f6d-10826299.jpg)  
                    
                
                -   변경가능![](https://api.transno.com/v3/document_image/a5e8c33e-8df8-4325-8c39-a0e9a90c2fc9-10826299.jpg)  
                    
        
        -   고유키  
            
            -   중복되지 않은 유일한 값  
                
            
            -   중복 X, null O, 여러개 설정  
                
        
        -   체크 제약조건  
            
            -   입력되는 데이터를 점검하는 기능  
                
            
            -   에시 -> 키를 100이상만 입력할 수 있도록 제약![](https://api.transno.com/v3/document_image/c37ef486-7cb6-4c8c-a475-53dbcafe404b-10826299.jpg)  
                
            
            -   벗어나서 입력하면?  
                
                -   입력된거 같지만(오류 안떠서) 입력안됌. 오류 발생하게 해보자  
                    
                
                -   with check option을 통해 범위값만 넣을 수 있도록 하자![](https://api.transno.com/v3/document_image/70d929d8-4f75-40f9-a7a1-2862d1b76efb-10826299.jpg)  
                    
        
        -   기본값  
            -   값을 입력하지 않을때 자동으로 입력되는 값  
                
        
        -   null  
            -   허용하면null, 아니면 not null  
                
-   Chapter 06 인덱스  
    
    -   클러스터형 인덱스란?  
        -   "사전"과 비슷한 개념으로, 기본키로 지정하면 자동 생성되며 테이블에 1개만  
            
    
    -   보조 인덱스란?  
        -   "찾아보기"와 비슷한 개념으로, 고유 키로 지정하면 자동 생성되며 여러개 만들 수 있지만, 자동 정렬 X  
            
    
    -   인덱스 장단점  
        
        -   장점  
            
            -   검색 속도 매우 빨라짐  
                
            
            -   시스템 성능 향상시킴  
                
        
        -   단점  
            -   데이터베이스 안에 추가적인 공간 필요함  
                
    
    -   내부 작동  
        
        -   인덱스 구성  
            
            -   인덱스 구성(클러스터형, 보조인데스 모두)은 균형트리 구조![](https://api.transno.com/v3/document_image/5dd1d0d4-af08-4465-8121-38f0d814358d-10826299.jpg)  
                노드 == 페이지
            
            -   루트 검색 후 조건 맞으면 리프 페이지로 이동  
                
        
        -   페이지 분할  
            
            -   처음 상태![](https://api.transno.com/v3/document_image/0845c38f-c967-4040-b257-f0727130cd7a-10826299.jpg)  
                
            
            -   2번째 리프에 인덱스를 추가한다면 어떤 매커니즘?![](https://api.transno.com/v3/document_image/af19b4b4-ce18-48da-a992-471775b15b7a-10826299.jpg)  
                
            
            -   루트 페이지가 꽉차면?![](https://api.transno.com/v3/document_image/f736dcd4-c779-47a0-b55c-0c8f0c1b59c6-10826299.jpg)  
                
        
        -   클러스터형 인덱스 찾기  
            -   그림![](https://api.transno.com/v3/document_image/2954d3b5-112e-431b-aa67-60339cb878a7-10826299.jpg)  
                클러스터형 인덱스는 키본키로, 자동정렬
        
        -   보조 인덱스 찾기  
            -   그림![](https://api.transno.com/v3/document_image/f77dff29-bb0f-4156-950e-cbd62b73b0b6-10826299.jpg)  
                보조 인덱스는 고유키로, 정렬 x 그래서 +#번호로 위치 나타내고 있음
    
    -   실제 사용  
        
        -   인덱스 생성/제거 구조  
            
            -   구조![](https://api.transno.com/v3/document_image/0c2561ca-035f-4a0a-bfa3-3ccc1fa178b7-10826299.jpg)  
                
            
            -   생성한 인덱스를 실제 적용시키려면 analyze table문으로 테이블 처리해줘야함.![](https://api.transno.com/v3/document_image/1bdbe915-ce8b-490f-bcb4-a7fc644eed32-10826299.jpg)  
                
        
        -   인덱스가 있는데 인덱스를 사용하지 않는 경우는?  
            
            -   1) 대부분의 행을 가져오는 경우 데이터를 차례대로 읽는 것이 효율적이라 mysql이 판단  
                
            
            -   2) where절 조건의 변형형을 줄 경우  
                
                -   인덱스 가능![](https://api.transno.com/v3/document_image/47e2b300-adee-43d6-a692-25329038fd8a-10826299.jpg)  
                    
                
                -   인덱스 불가능![](https://api.transno.com/v3/document_image/ad6dc923-4815-4816-b1f9-96dcef2a7d39-10826299.jpg)  
                    
        
        -   인덱스 효과적으로 사용하는 방법  
            
            -   인덱스는 하나의 열단위로 생성하자  
                
            
            -   where절에서 자주 사용되는 열에 인덱스를 만들자  
                
            
            -   카디널리티(=중복이 높은 경우) 만들어도 효과 X  
                
            
            -   사용하지 않는 인덱스 삭제하기