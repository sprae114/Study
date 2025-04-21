# 07장 AWS에 데이터베이스 환경을 만들어보자 - AWS RDS  
#### 📌1 RDS 인스턴스 생성하기  
7.2 RDS 운영환경에 맞는 파라미터 설정하기  
7.3 내 PC에서 RDS에서 접속해 보기  
Database 플러그인 설치  
7.4 EC2에서 RDS에서 접근 확인  


# 08장 EC2 서버에 프로젝트를 배포해 보자  
8.1 EC2에 프로젝트 Clone 받기  
8.2 배포 스크립트 만들기  
## 3 외부 Security 파일 등록하기  
#### 📌이전 실행에서의 문제점
=> 깃에서 클론을 하고 스크립트 짜서 실행을 했더니 `Client Registration Repository` 오류가 발생했다. 그 이유는 우리가 깃허브에 올릴때, ignore을 통해서 git에 제외대상이 있기 때문임. 그래서 따로 properties를 만들어줘야함.

#### 📌properties 설정하기
1) vim으로 properties 만들기
```
//properties만든후, 로컬에 있는 properties 내용 복붙하기
vim /home/ec2-user/app/application-oauth.properties
```

2) 스크립트에 다음 내용 적용하기
```
nohup java -jar \ -Dspring.config.location=classpath:/application.properties,/home/ec2-user/app/application-oauth.properties \ $REPOSITORY/$JAR_NAME 2>&1 &
```

## 4 스프링 부트 프로젝트로 RDS 접근하기  
#### 📌RDS 테이블 생성  

#### 📌프로젝트 설정  

#### 📌EC2 설정 

#### 📌EC2에서 소셜 로그인하기  


# 09장 코드가 푸시되면 자동으로 배포해 보자 - Travis CI 배포 자동화  
9.1 CI & CD 소개  
9.2 Travis CI 연동하기  
Travis CI 웹 서비스 설정  
프로젝트 설정  
9.3 Travis CI와 AWS S3 연동하기  
AWS Key 발급  
Travis CI에 키 등록  
S3 버킷 생성  
.travis.yml 추가  
9.4 Travis CI와 AWS S3, CodeDeploy 연동하기  
EC2에 IAM 역할 추가하기  
CodeDeploy 에이전트 설치  
CodeDeploy를 위한 권한 생성  
CodeDeploy 생성  
Travis CI, S3, CodeDeploy 연동  
9.5 배포 자동화 구성  
deploy.sh 파일 추가  
.travis.yml 파일 수정  
appspec.yml 파일 수정  
실제 배포 과정 체험  
9.6 CodeDeploy 로그 확인