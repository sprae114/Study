-   03장 스프링 부트에서 JPA로 데이터베이스 다뤄보자  
    
    -   3.1 JPA 소개  
        
        -   Spring Data JPA  
            -   JPA가 등장한 이유?  
                
                -   구현체 교체의 용이성  
                    
                
                -   저장소 교체의 용이성  
                    
        
        -   실무에서 JPA  
            -   jpa 장점/ 단전  
                
                -   장점  
                    -   CRUD 쿼리를 직접 작성할 필요가 x  
                        
                
                -   단점  
                    -   높은 러닝커브  
                        
        
        -   요구사항 분석  
            -   구현할 기능![](https://api.transno.com/v3/document_image/732ad5ce-a513-4417-8a39-3a36ee4bd7af-10826299.jpg)  
                
    
    -   3.2 프로젝트에 Spring Data Jpa 적용하기  
        
        -   1) 의존성 등록하기  
            -   의존성 등록![](https://api.transno.com/v3/document_image/55825298-3521-42b0-88a3-910646e0996e-10826299.jpg)  
                
        
        -   2) 도메인을 담을 패키지  
            
            -   domain?  
                -   게시글, 댓글 등 소프트웨어에 대한 요구사항 혹은 문제 영역  
                    
            
            -   domain/posts/Posts  
                
                -   코드![](https://api.transno.com/v3/document_image/20f8591d-a645-4d8e-916f-cbb757ede23b-10826299.jpg)  
                    
                
                -   몰랐던 내용![](https://api.transno.com/v3/document_image/d25fcdd9-b691-4ae9-a3e1-9e1be339d218-10826299.jpg)![](https://api.transno.com/v3/document_image/c3441dc4-a939-413f-8e3e-11a33315f790-10826299.jpg)  
                    
            
            -   domain/posts/PostsRepository  
                -   코드![](https://api.transno.com/v3/document_image/facacf4f-3658-4d0c-bbc4-7d5b7fd1e813-10826299.jpg)  
                    
        
        -   추가 내용  
            
            -   [스프링부트 h2 설정](https://bgpark.tistory.com/78)  
                
            
            -   [[JPA] 기본키(PK) 매핑 방법 및 생성 전략](https://gmlwjd9405.github.io/2019/08/12/primary-key-mapping.html)  
                
    
    -   3.3 Spring Data JPA 테스트 코드 작성하기  
        
        -   h2를 통해 데이터 베이스와 연결  
            
        
        -   posts/PostsRepositoryTest  
            
            -   코드![](https://api.transno.com/v3/document_image/e787fef3-d7d9-47e7-a033-06729bab7835-10826299.jpg)![](https://api.transno.com/v3/document_image/06a464d0-06fe-44bf-99b0-14407183a8c8-10826299.jpg)  
                
            
            -   몰랐던 내용  
                -   BaseTime 테스트 코드 작성 방법  
                    
    
    -   3.4 등록/수정/조회 API 만들기  
        
        -   [Spring 웹 계층](https://leveloper.tistory.com/14)  
            
        
        -   API를 만들때 필요한 클래스 3개  
            
            -   1) Request 데이터를 받을 Dto  
                
            
            -   2) API 요청을 받을 Controller  
                
            
            -   3) 트랜잭션, 도메인 기능 간의 순서를 보장하는 Service  
                -   Service 역할  
                    -   비즈니스 로직 처리 x => 트랜잭션, 도메인 간의 순서 보장역할만  
                        
        
        -   API 직접 만들기  
            
            -   Request와 Response용 Dto는 view를 위한 클래스라 자주 변경되로 반드시 분리하기  
                
            
            -   web/PostsApiController  
                -   코드![](https://api.transno.com/v3/document_image/4b4aef70-255c-4597-94a6-a8693a127f1c-10826299.jpg)  
                    
            
            -   service/PostsService  
                -   코드![](https://api.transno.com/v3/document_image/a079b0f1-a2bf-4920-8dc5-4deedcc42f32-10826299.jpg)  
                    
            
            -   web/dto/PostsSaveRequestDto  
                -   코드![](https://api.transno.com/v3/document_image/c0675cd3-c014-4cdd-a659-74155c1d8428-10826299.jpg)  
                    
            
            -   web/dto/PostsSaveRespeonseDto  
                -   코드![](https://api.transno.com/v3/document_image/c4044ad9-dcb4-4973-9582-f9ed35c5e6f3-10826299.jpg)  
                    
            
            -   web/dto/PostsUpdateRequestDto  
                -   코드![](https://api.transno.com/v3/document_image/e4ac12e6-9fe4-4a09-96ce-ce63893a3a26-10826299.jpg)  
                    
            
            -   web/PostsApiControllerTest  
                -   코드![](https://api.transno.com/v3/document_image/a5936424-d8bf-47df-8731-6bd818894cd0-10826299.jpg)![](https://api.transno.com/v3/document_image/91a680c5-53d1-4c45-bcec-66e682e99ed3-10826299.jpg)![](https://api.transno.com/v3/document_image/c1bd4d3d-3d6f-423c-bd4c-4c64f0bb570b-10826299.jpg)  
                    
    
    -   3.5 JPA Auditing으로 생성시간/수정시간 자동화하기  
        
        -   Entity 에는 해당 데이터의 생성시간과 수정시간을 포함 -> 이 코드를 줄여주는 역할  
            
        
        -   BaseTimeEntity 설정하는 법  
            
            -   ~Application에 추가![](https://api.transno.com/v3/document_image/715effae-0c17-4f5c-a29c-1f97d6730d11-10826299.jpg)  
                
            
            -   Entity 클래스 상속![](https://api.transno.com/v3/document_image/f2ba42bc-2e13-4605-b8d3-439a9dc20321-10826299.jpg)  
                
        
        -   domain/BaseTimeEntity  
            
            -   코드![](https://api.transno.com/v3/document_image/6a9da210-a387-4225-b0e2-5e9596fb3cb0-10826299.jpg)  
                
            
            -   몰랐던 내용  
                -   BaseTimeEntity 사용법![](https://api.transno.com/v3/document_image/838c9aba-9074-4a78-9718-bd9cfe9591a1-10826299.jpg)![](https://api.transno.com/v3/document_image/5b72e39d-4531-4a79-aa13-18f0d0c28ed4-10826299.jpg)  
                    
        
        -   BaseTimeEntity 테스트 코드  
            -   코드![](https://api.transno.com/v3/document_image/f0b7b944-6482-43ef-835d-82acb8acee78-10826299.jpg)  
                
-   04장 머스테치로 화면 구성하기  
    
    -   4.1 서버 템플릿 엔진과 머스테치 소개  
        
        -   템플릿 엔진?  
            -   지정된 펨플릿 양식과 데이터가 합쳐저 HTML문서를 출력하는 소프트웨어  
                
        
        -   서버 템플릿 엔진 (Ex JSP, Freemarker)![](https://api.transno.com/v3/document_image/2eb9154b-daa4-4ad5-b3b7-8f108cf8537c-10826299.jpg)  
            -   화면 생성은 서버에서 java 코드로 문자열을 만든 뒤 이 문자열을 HTML로 변환하여 브라우저로 전달  
                
        
        -   클라이언트 템플림 엔진(Ex react, view)![](https://api.transno.com/v3/document_image/4f4628a3-737b-4223-9ee8-c9a52da57b52-10826299.jpg)  
            
        
        -   머스테치란  
            
            -   가장 간단한 템플릿 엔진  
                
            
            -   view의 역할과 서버의 역할이 명확하게 나뉨  
                
            
            -   하나의 문법으로 클라이언트/서버 템플릿 모두 사용가능  
                
    
    -   4.2 기본 페이지 만들기  
        
    
    -   4.3 게시글 등록 화면 만들기  
        
    
    -   4.4 전체 조회 화면 만들기  
        
    
    -   4.5 게시글 수정, 삭제 화면 만들기  
        
        -   게시글 수정  
            
        
        -   게시글 삭제  
            
-   05장 스프링 시큐리티와 OAuth 2.0으로 로그인 기능 구현하기  
    
    -   OAuth  
        
        -   5.1 스프링 시큐리티와 스프링 시큐리티 Oauth2 클라이언트  
            -   소셜 로그인 사용하는 이유는?  
                -   구현해야 될 기술이 많아서 시간이 오래걸림![](https://api.transno.com/v3/document_image/1fbfabda-33c9-452c-95b9-ef9a3e1948b9-10826299.jpg)  
                    
        
        -   5.2 구글 서비스 등록  
            
        
        -   5.3 구글 로그인 연동하기  
            
        
        -   5.6 네이버 로그인  
            
            -   네이버 API 등록  
                
            
            -   스프링 시큐리티 설정 등록  
                
        
        -   로그인 기능  
            -   [OAuth 란 무엇일까 · Showerbugs](https://showerbugs.github.io/2017-11-16/OAuth-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)  
                
    
    -   스프링 시큐리티  
        -   스프링 시큐리티 설정  
            
            -   의존성 추가하기  
                -   security 의존성![](https://api.transno.com/v3/document_image/502a4331-01e8-493f-8e05-faced49266ef-10826299.jpg)  
                    
            
            -   config/auth/SecurityConfig  
                
                -   코드![](https://api.transno.com/v3/document_image/136813a8-c3a7-4a5c-bf03-00c392ef0b13-10826299.jpg)  
                    
                
                -   설명![](https://api.transno.com/v3/document_image/4545a961-cf4e-42ae-8177-dee4fbd21443-10826299.jpg)![](https://api.transno.com/v3/document_image/e8eb0389-8a5a-4130-953e-82336ba50113-10826299.jpg)  
                    
    
    -   5.4 어노테이션 기반으로 개선하기(p.195)  
        
        -   세션값을 가져오는 부분 어노테이션 기반으로 변경해보자  
            
        
        -   config/auth/LoginUser  
            
            -   코드![](https://api.transno.com/v3/document_image/ac0d9d1e-ddaf-4b26-a2e0-326dac54079a-10826299.jpg)  
                
            
            -   설명![](https://api.transno.com/v3/document_image/efdfd13a-9086-4dc7-8072-2846ab1860af-10826299.jpg)  
                
        
        -   config/auth/LoginUserArgumentResolver  
            
            -   코드![](https://api.transno.com/v3/document_image/2c97bb43-0acf-4a95-8571-d65e1a3d60c8-10826299.jpg)  
                
            
            -   설명![](https://api.transno.com/v3/document_image/1a292a82-5c4b-4531-aa88-cd446a3fcc24-10826299.jpg)![](https://api.transno.com/v3/document_image/58dbc81e-9d5e-4b10-a77d-0b615ee59fea-10826299.jpg)  
                
        
        -   config/WebConfig  
            -   코드![](https://api.transno.com/v3/document_image/e25a28c2-5e7f-41a8-a12b-1a469fb5859b-10826299.jpg)  
                
        
        -   web/IndexController 에 적용하기  
            
            -   코드![](https://api.transno.com/v3/document_image/f7717f81-1dac-4a06-aa09-53f44d1372c8-10826299.jpg)  
                
            
            -   설명![](https://api.transno.com/v3/document_image/03e115f1-d9e8-4ec2-a4ba-ef5b812da171-10826299.jpg)  
                
    
    -   5.5 세션 저장소로 데이터베이스 사용하기(p.200)  
        
        -   세션 저장소 3가지 방법  
            -   설명![](https://api.transno.com/v3/document_image/9b2842c5-3761-48b9-a26b-f694139f0970-10826299.jpg)  
                
        
        -   설정  
            -   의존성 등록  
                
                -   gradle![](https://api.transno.com/v3/document_image/00da991d-5e95-412b-a8a7-532f5fc830b8-10826299.jpg)  
                    
                
                -   properties![](https://api.transno.com/v3/document_image/7fe3694f-3e8a-4746-90e5-931b585ddab3-10826299.jpg)  
                    
    
    -   5.7 기존 테스트에 시큐리티 적용하기(p.211)  
        -   기존 테스트에 시큐리티 적용하면 오류 발생  
            
            -   1) CustomOAuthUserService을 찾을 수 없음(p.213)  
                
            
            -   2) @ WebMvcTest에서 CustomOAuthUserService을 찾을 수 없음(p.219)  
                
    
    -   스프링 시큐리티  
        
        -   왜 쓰는 거야?  
            -   막강한 인증과 권한 부여 기능을 가진 프레임 워크로 보안을 위해 사용  
                
        
        -   인터셉터나 필터 기반이 아닌 스프링 시큐리티를 사용하는 이유는?  
            
            -   확장상성을 고려할 수 있엉 손 쉽게 추가 변경 가능  
                
            
            -   [[Spring] 필터(Filter) vs 인터셉터(Interceptor) 차이 및 용도 - MangKyu's Diary (tistory.com)](https://mangkyu.tistory.com/173)  
                
-   나중에  
    
    -   06장 AWS 서버 환경을 만들어보자 - AWS EC2  
        -   aws 클라우드 서비스를 이용해 서버 배포  
            -   서버 작동시키는 3가지 방법  
                
                -   집에서 pc를 24시간 구동  
                    
                
                -   호스팅 서비스 이용  
                    
                
                -   클라우드 서비스 이용  
                    
    
    -   07장 AWS에 데이터베이스 환경을 만들어보자 - AWS RDS  
        
        -   데이터 베이스 구축 및 EC2 서버와 연동  
            
        
        -   AWS에서 제공하는 RDS 사용 why? 데이터 베이스 설정 및 운영 작업을 자동화하여 개발에 집중할 수 있게함  
            
        
        -   DB 엔진 : MariaDB  
            -     
                
    
    -   08장 EC2 서버에 프로젝트를 배포해 보자  
        
        -   _8.1 EC2에 프로젝트 Clone 받기  
            
        
        -   _8.2 배포 스크립트 만들기  
            
        
        -   _8.3 외부 Security 파일 등록하기  
            
        
        -   _8.4 스프링 부트 프로젝트로 RDS 접근하기  
            
        
        -   __RDS 테이블 생성  
            
        
        -   __프로젝트 설정  
            
        
        -   __EC2 설정  
            
        
        -   _8.5 EC2에서 소셜 로그인하기  
            
    
    -   09장 코드가 푸시되면 자동으로 배포해 보자 - Travis CI 배포 자동화  
        
        -   _9.1 CI & CD 소개  
            
        
        -   _9.2 Travis CI 연동하기  
            
        
        -   __Travis CI 웹 서비스 설정  
            
        
        -   __프로젝트 설정  
            
        
        -   _9.3 Travis CI와 AWS S3 연동하기  
            
        
        -   __AWS Key 발급  
            
        
        -   __Travis CI에 키 등록  
            
        
        -   __S3 버킷 생성  
            
        
        -   __.travis.yml 추가  
            
        
        -   _9.4 Travis CI와 AWS S3, CodeDeploy 연동하기  
            
        
        -   __EC2에 IAM 역할 추가하기  
            
        
        -   __CodeDeploy 에이전트 설치  
            
        
        -   __CodeDeploy를 위한 권한 생성  
            
        
        -   __CodeDeploy 생성  
            
        
        -   __Travis CI, S3, CodeDeploy 연동  
            
        
        -   _9.5 배포 자동화 구성  
            
        
        -   __deploy.sh 파일 추가  
            
        
        -   __.travis.yml 파일 수정  
            
        
        -   __appspec.yml 파일 수정  
            
        
        -   __실제 배포 과정 체험  
            
        
        -   _9.6 CodeDeploy 로그 확인  
            
    
    -   10장 24시간 365일 중단 없는 서비스를 만들자  
        
        -   _10.1 무중단 배포 소개  
            
        
        -   _10.2 엔진엑스 설치와 스프링 부트 연동하기  
            
        
        -   _10.3 무중단 배포 스크립트 만들기  
            
        
        -   __profile API 추가  
            
        
        -   __real1, real2 profile 생성  
            
        
        -   __엔진엑스 설정 수정  
            
        
        -   __배포 스크립트들 작성  
            
        
        -   _10.4 무중단 배포 테스트  
            
    
    -   11장 1인 개발 시 도움이 될 도구와 조언들  
        
        -   _11.1 추천 도구 소개  
            
        
        -   __댓글  
            
        
        -   __외부 서비스 연동  
            
        
        -   __방문자 분석  
            
        
        -   __CDN  
            
        
        -   __이메일 마케팅  
            
        
        -   _11.2 1인 개발 팁  
            
        
        -   _11.3 마무리  
            
-   발생한 오류들  
    
    -   Error - Port 8080 was already in use  
        -   [Error - Port 8080 was already in use (tistory.com)](https://7942yongdae.tistory.com/35)  
            
    
    -   [[IntelliJ] java.lang.IllegalStateException: Failed to load ApplicationContext :: IT 머릿속의 메모장 (tistory.com)](https://lightchan.tistory.com/157)  
        
    
    -   [AWS RDS 사용 시, DB Navigator에러 | DeCoding (xengom.com)](https://xengom.com/aws/aws-rds-db-navigator/)