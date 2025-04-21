
# 8.데이터베이스 서버 준비하기  
#### 8장에서 만들고자 하는것은?
여태까지는 인터넷에서 접근할 수 있는 인프라를 만들었다. 이제는 **데이터베이스를 연결**해보자.
![image](https://github.com/sprae114/Study/assets/52237184/b98b07b4-b667-45f4-9ac1-3f765c14e2fa)


#### 데이터베이스 서버란?
데이터베이스 관리 시스템(DBMS)이 설치되어 있는 서버로, **데이터를 안전하게 보관, 조직, 검색 및 수정할 수 있는 저장소**를 제공하는 컴퓨터 시스템입니다.


#### RDMS이란?
- **참조 관계의 여러 테이블로 데이터 관리**
- SQL이라는 전용 질의 언어로 데이터 입출력 수행
![image](https://github.com/sprae114/Study/assets/52237184/4a381b5e-35c9-4c67-ba86-3653bfe679c6)


#### RDS란?
AWS에서 제공하는 **완전 관리형 관계형 데이터베이스 서비스**입니다. RDS는 여러 관계형 데이터베이스 관리 시스템 (RDBMS)을 지원하며, 데이터베이스 구축, 운영, 및 확장을 용이하게 하는 서비스입니다.
![image](https://github.com/sprae114/Study/assets/52237184/94205d77-2903-4ca3-a202-903d4ebd5c68)


#### RDS를 사용하는 이유는?
Amazon RDS를 사용하는 것이 EC2 인스턴스에 직접 데이터베이스를 설치하는 것보다 여러 가지 이유로 더 효율적이고 편리합니다. RDS를 사용하면 다음과 같은 이점이 있습니다.

1. 관리의 편리성: RDS는 관리형 서비스로써, 데이터베이스의 하드웨어 관리, 설정, 소프트웨어 패치, 백업 및 모니터링 등과 같은 작업들을 자동화하고 관리해줍니다. 이로 인해 사용자는 이러한 작업에 시간을 들이지 않아도 되며, 대신 애플리케이션 개발 및 운영에 집중할 수 있습니다.

2. 확장성: Amazon RDS는 사용자 요구에 따라 스토리지, 컴퓨팅 리소스 및 메모리를 쉽게 확장할 수 있는 확장성을 제공합니다. 이처럼 수월한 확장성 덕분에 비즈니스가 성장함에 따라 데이터베이스 용량을 빠르게 조절할 수 있습니다.

3. 고가용성: RDS는 Multi-AZ 배포를 통해 기본 인스턴스와 예비 인스턴스 간의 자동 장애 조치를 지원합니다. 이로 인해 서비스 중단 시간이 최소화되며 가용성이 향상됩니다.

4. 백업 및 복구: Amazon RDS는 자동 백업, 스냅샷 백업 및 포인트-인-타임 복구를 지원하여 데이터 손실 위험을 최소화하고, 장애 발생 시 빠르게 데이터를 복구할 수 있습니다.

5. 보안: VPC, 보안 그룹, 서브넷 그룹 및 인스턴스에 대한 엄격한 보안 설정, 데이터 암호화, SSL/TLS 연결 등을 통해 데이터베이스 보안을 강화할 수 있습니다.


#### RDS 시스템의 구성요소는?
![image](https://github.com/sprae114/Study/assets/52237184/0295c560-84b5-4aa1-a978-a58ed4d1f4c8)

1. 데이터베이스 엔진: 데이터베이스 엔진은 데이터베이스 관리 시스템(DBMS)의 핵심으로서 **데이터 처리**, 저장, 검색 및 삭제와 같은 업무를 수행합니다. Amazon RDS는 다양한 관계형 데이터베이스 엔진을 지원하며, 흔히 사용되는 엔진에는 MySQL, PostgreSQL, ..  있습니다.

2. 파라미터 그룹: 파라미터 그룹은 데이터베이스 **엔진 설정**에 사용되는 매개 변수들의 모음입니다. 이러한 매개 변수들은 데이터베이스 엔진의 메모리, 쿼리 최적화 설정, 인스턴스 설정 등에 영향을 줍니다. 파라미터 그룹을 사용하여 데이터베이스 인스턴스의 성능을 조정하고, 요구사항에 맞게 엔진 구성을 사용자 정의할 수 있습니다.

3. 옵션 그룹: 옵션 그룹은 Amazon RDS에서 선택적으로 사용할 수 있는 **기능들을 관리**하기 위한 컨테이너입니다. 각 데이터베이스 엔진에 따라 서로 다른 옵션 그룹을 정의할 수 있습니다. 예를 들어, 데이터베이스 모니터링 및 로깅, 인덱싱, 보안과 같은 추가 기능을 제공하는 옵션들을 정의하고 관리할 수 있습니다.

4. 서브넷 그룹: 서브넷 그룹은 Amazon Virtual Private Cloud(VPC)에서 실행되는 Amazon RDS 데이터베이스 인스턴스를 사용해 각각의 **서브넷을 관리**할 수 있는 컨테이너입니다. 데이터베이스 인스턴스가 실행될 VPC를 정의하고, VPC의 서브넷 간에 인스턴 배포 방식을 구성할 수 있습니다. 서브넷 그룹을 사용하면 특정 데이터베이스 인스턴스를 가용 영역(AZ) 간에 분산하거나, 다중 AZ 배포를 설정하여 높은 가용성을 지원할 수 있습니다.


#### 파라미터 그룹 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/17df0de7-824c-4f44-a9a7-0015b8c77f6e)


#### 옵션 그룹 생성 내용 
![image](https://github.com/sprae114/Study/assets/52237184/11923ba2-cb9a-48a6-959b-4d4504e8a2fe)


#### 서브넷 그룹 생성 내용 
![image](https://github.com/sprae114/Study/assets/52237184/1e173a8e-f74f-4cef-adf3-ff42da12e9dd)


#### 데이터베이스 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/29e63ba4-45b5-4a2f-a967-c315ec89d967)



# 9.이미지 저장 장소 준비하기  
#### 9장에서 만들고자 하는것은?
이미지처럼 **크기가 큰 정보를 저장**해야함. 데이터베이스에 들어갈 수 없으므로 전용 장소(S3)에 저장하자.
![image](https://github.com/sprae114/Study/assets/52237184/fbf4a862-245f-475b-838b-749872f23d6d)


#### 스토리지란?
데이터를 오랫동안 저장하는 것을 목적으로 제공되는 데이터 저장 장소임.
![image](https://github.com/sprae114/Study/assets/52237184/2c8e2055-ebd1-434d-bb9a-c470e4176720)


#### 스토리지 vs 메모리
![image](https://github.com/sprae114/Study/assets/52237184/c422ba74-933d-4d41-aafc-c5d0a3a6495f)


#### S3 vs EBS
▶Amazon S3
- 객체 저장소: S3는 **객체 기반 스토리지**로, 이미지나 동영상과 같은 객체를 저장하고 검색할 수 있습니다. S3는 **키와 값의 형태로 데이터를 저장**합니다.
- 확장성: S3는 거의 무제한의 용량을 제공하며 사용자가 원하는 만큼 스토리지를 확장할 수 있습니다.
- 지속성: S3는 기본적으로 자동 복제 기능이 포함되어 있어 데이터의 내구성이 높습니다.
- 웹 앱 또는 인터넷상의 콘텐츠 저장 및 배포: 정적 웹 사이트 호스팅, 백업, 애플리케이션 데이터, 빅 데이터 분석 등에 적합합니다.

▶Amazon EBS
- 블록 스토리지: EBS는 블록 스토리지로서, **가상 서버(EC2 인스턴스)와 연결되어 사용**됩니다. EBS 볼륨은 가상 디스크처럼 작동하며, **파일 시스템 또는 데이터베이스에 사용되는 스토리지**입니다.
- 지속성: EBS는 자동 복제 기능이 제한적이므로, 데이터 내구성을 높이려면 추가 스냅샷이나 백업 정책이 필요합니다.
- 서버 연결에 적합: EBS는 EC2 인스턴스와 통합된 스토리지로 사용되기 때문에, 인스턴스에서 실행되는 애플리케이션, 데이터베이스, 파일 시스템 등에 적합합니다.
![image](https://github.com/sprae114/Study/assets/52237184/11d0d576-4d92-45f4-ac08-82b08963c54a)


#### S3와 VPC의 관계 
S3는 VPC 밖에 생성함.
![image](https://github.com/sprae114/Study/assets/52237184/56cd80b5-a9a8-4452-9b1e-c5386fdbe943)

그래서 VPC 안의 리소스로 접근할때는 S3 버킷에 대한 접근 권한이 필요함. IAM의 역할 개념을 이용해 역할을 적용함.
![image](https://github.com/sprae114/Study/assets/52237184/eb94b39d-62fa-40f1-9aad-5aefd5d1e0f1)


#### S3 시스템
버킷은 S3에서 관리하는 데이터를 하나로 모은 단위다. 버킷 안에는 서비스에서 이용하는 데이터인 객체(EX텍스트, 이미지, ...)를 저장함. 대량의 객체가 있을 때는 폴더를 이용해서 관리함.
![image](https://github.com/sprae114/Study/assets/52237184/942d75f7-3c7e-404c-af30-5d4be40ae1ef)


#### S3 버킷 생성내용
![image](https://github.com/sprae114/Study/assets/52237184/ac363a4f-5885-4fdf-bb19-f036c33c95c8)


#### 역할을 생성해 EC2에 적용하기 생성내용
![image](https://github.com/sprae114/Study/assets/52237184/b2cbec1a-7668-4be5-b7dd-5dc2c0ff5cf9)


# 10.커스텀 도메인과 DNS 준비하기  
#### 10장에서 만들고자 하는것은?
![image](https://github.com/sprae114/Study/assets/52237184/6a1a5867-42cb-4598-a14d-22e70d6993e6)


#### DNS
인터넷에서 도메인 이름을 IP 주소로 변환하는 시스템임.
1) DNS 서버에서 IP 주소가 있는 경우
![image](https://github.com/sprae114/Study/assets/52237184/13546a68-3021-4fad-9798-6665a116d85d)

2) DNS 서버에서 IP주소가 없는 경우
![image](https://github.com/sprae114/Study/assets/52237184/35a318dd-7eba-44ec-9b30-1fa5dffd926c)


#### SSL 서버 인증서 
클라이언트와 서버 간의 통신이 암호화되어 안전하게 이루어지도록 하는 디지털 인증서입니다. SSL 인증서를 사용하면 사이트의 신원을 확인할 수 있고, 클라이언트와 서버 간의 통신이 안전하게 이루어지도록 보장합니다.
![image](https://github.com/sprae114/Study/assets/52237184/c1cb0a66-2fab-42c7-ac7a-438b331b17b6)

![image](https://github.com/sprae114/Study/assets/52237184/f348efcb-4653-4029-a2df-819dc3eafd08)


#### Route 53의 기능은?
1) 도메인 이름 등록
상위 도메인을 관리하는 조직에 자신의 도메인을 요청해서 등록하는 것
![image](https://github.com/sprae114/Study/assets/52237184/d08243ab-37a0-429d-92a2-bd2f470f6af5)


2) DNS 서버
▶퍼블릭 DNS 
외부에 공개하는 DNS로, 퍼블릭 DNS를 이름 결정에 따라 퍼블릭 IP를 반환함.
![image](https://github.com/sprae114/Study/assets/52237184/0e007a16-3781-4e39-9496-52952c96d013)

▶프라이빗 DNS
외부에 공개하지 않는 DNS로, 시스템 내부에 생성한 리소스에 이름을 붙여 관리할때 사용함.
![image](https://github.com/sprae114/Study/assets/52237184/0a805e0d-20ed-41b1-9448-322b5c8bf857)

![image](https://github.com/sprae114/Study/assets/52237184/35b774b5-3b1b-410f-91f2-2996182aed6a)


#### 도메인 이름 취득하기 생성 내용 
![image](https://github.com/sprae114/Study/assets/52237184/180e80d8-c7b4-4e9c-8383-acb3934e8105)


#### 퍼블릭 DNS에 리소스 정보 추가하기 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/6521f076-44e8-4f34-aad5-5a47a37fac6f)

![image](https://github.com/sprae114/Study/assets/52237184/d89086db-5562-4b50-b4d1-3a3cfdf4b4a8)


#### 프라이빗 DNS 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/a1e5d61d-ec16-495d-8291-effa9e813724)


# 11.메일 서버 준비하기

#### 11장에서 만들고자 하는것은?
![image](https://github.com/sprae114/Study/assets/52237184/f26cf9d0-8cfa-4992-8749-dacc5b155550)


#### 기존 메일 시스템
![image](https://github.com/sprae114/Study/assets/52237184/bec4bbfe-6261-431e-a23d-46e9ec3ca7a6)

![image](https://github.com/sprae114/Study/assets/52237184/1b11824d-666c-4d2c-8db1-c4abea13283f)


#### Amazon SES
![image](https://github.com/sprae114/Study/assets/52237184/78aa41ef-cad9-482f-9041-7ccfa3840739)

![image](https://github.com/sprae114/Study/assets/52237184/31a8b433-3622-42cc-af74-2ac562246e71)


# 12.캐시 서버 준비하기  
#### 12장에서 만들고자 하는것은?
캐시서버를 사용해서 성능향상을 시켜보자
![image](https://github.com/sprae114/Study/assets/52237184/bc89cbc3-cf0a-4cec-a3be-1d5909593088)

#### 캐시 서버란?
처리에 시간이 걸리는 결과 데이터를 저장해두고, 다음에 같은 처리를 수행할 때는 저장된 결과 데이터를 사용해 결과를 빠르게 반환하는 시스템
![image](https://github.com/sprae114/Study/assets/52237184/ddee2288-4ed7-43ad-8273-d57aaa870c5b)


#### 캐시 사용시 주의할 점은?
1) 캐시 데이터의 노후화
![image](https://github.com/sprae114/Study/assets/52237184/ebc80dd0-6e60-4915-9578-004d384f6e86)

2) 캐시 저장 영역이 필요
![image](https://github.com/sprae114/Study/assets/52237184/450fed0b-63d3-4cae-9fa5-c92c73abc335)

이러한 점을 해결하기 위해, **캐시에 저장할 데이터에는 보통 유효기간을 설정**함.
![image](https://github.com/sprae114/Study/assets/52237184/58fbe2a5-36d8-48b1-97d6-8bbf75beda8c)


#### 일래스틱캐시
(AWS)에서 제공하는 **메모리 기반의 분산 데이터 저장 및 캐싱 솔루션**입니다. ElastiCache는 두 가지 캐싱 엔진, 즉 Redis와 Memcached를 지원합니다.
기본적으로 임의의 키에 대해 캐시된 데이터를 반환하는 간단한 키/값 시스템을 제공함.
![image](https://github.com/sprae114/Study/assets/52237184/77a962f6-acb0-4a63-9d0f-7bdc8eb96fd0)


#### 일래스틱캐시 계층 시스템
- 노드 : 최소 단위. 실제 데이터는 노드에 저장. 각 노드는 독립적인 CPU, 메모리 및 네트워크 리소스를 갖고 있습니다. 이를 통해 요청된 데이터를 빠르게 처리할 수 있습니다.
- 샤드(노드 그룹) : 노드를 묶은 그룹. 하나의 프라이머리 노드와 여러 복제 노드로 구성. 복제 노드는 프라이머리 노드에 수행한 업데이트 내용이 복제되어 동일한 상태가 유지됌.
- 클러스터(복제 그룹) : 샤드를 묶은 그룹. 여러 샤드로 구성. 클러스터별로 독립적인 데이터를 관리하고 시스템에서 분리되어 작동합니다.
![image](https://github.com/sprae114/Study/assets/52237184/60ca7911-d542-4a15-a142-63e1a0204e1b)

![image](https://github.com/sprae114/Study/assets/52237184/258efe80-48a4-49dc-8ef0-f07e3d0b84aa)


#### 일래스틱캐시 생성 내용 
![image](https://github.com/sprae114/Study/assets/52237184/4b70f59b-f65a-4e05-9dac-0be6f2e28f32)

  
# 13.샘플 애플리케이션 작동하기  
#### 13장에서 만들고자 하는것은?
![[13-1.png]]
![[13-2.png]]
![[13-3.png]]
![[13-4.png]]

## 인스트럭처 구성
![[13-5.png]]
![[13-6.png]]


## 미들웨어 설치
엔진엑스 본체와 루비 온 레일즈를 작동하는 데  필요한 미들웨어 설치
```BASH
sudo yum -y install git gcc-c++ glibc-headers openssl-devel readline libyaml-devel

sudo amazon-linux-extras install -y nginx1
```


## 엔진엑스 설정
엔진 엑스가 루비 온 레일즈와 연동되도록 설정해야함.
#### 파일 편집기 생성
```BASH
sudo vim /etc/nginx/conf.d/rails.conf
```


#### 파일 편집
```config
upstream puma {
	#puma 설정으로 지정한 socket 파일 지정
	server unix:///var//www/aws-intro-sample/tmp/sockets/puma.sock;
}

server {
	#엔진엑스가 리스닝할 포트 설정
	listen 3000 default_server;
	listen [::]:300 default_server;
	
	server_name puma;
	
	location ~^/assets/ {
	root /var/www/aws-intro-sample/public;
	}

	location / {
		proxy_read_timeout 300; 
		proxy_connect_timeout 300; 
		proxy_redirect off;
		proxy_set_header Host $host;
		proxy_set_header X-Forwarded-Proto $http_x_forwarded_proto; 
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
		
		#위 server_name에서 설정한 이름으로 지정
		proxy_pass http://puma;
		}
}
```


## 루비 온 레일즈 환경 구축
#### deploy 사용자로 전환
```BASH
sudo su - deploy
```


#### 루비 설치
```BASH
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

source ~/.bash_profile

rbenv install 2.6.6

rbenv global 2.6.6
```


#### 루비 온 레일즈 설치
```BASH
gem install rails -v 5.1.6
```


## DB 생성
#### 데이터베이스와 사용자 생성
```BASH
$ mysql -u admin -p -h db.home
(여기에서 마스터 암호를 넣고 연결 시 정보가 표시됨) 

mysql> create database sample_app;
mysql> create user sample_app identified by '비밀번호 입력';
mysql> grant all privileges on sample_app.* to sample_app@'%';
mysql> quit
```


#### 샘플 애플리케이션 다운
```BASH
cd /var/www 
git clone https://github.com/moseskim/aws-intro-sample.git
```


#### Ruby 라이브러리 설치
```BASH
cd aws-intro-sample/ 
bundle install
```


#### 시크릿 키 생성
```BASH
rails secret
```


#### 샘플 애플리케이션 설정
```BASH
#샘플 애플리케이션용 설정 
export SECRET_KEY_BASE={생성한 시크릿키}
export AWS_INTRO_SAMPLE_DATABASE_PASSWORD={생성한 비밀번호}
export AWS_INTRO_SAMPLE_HOST={로드 밸린서에 붙인 CNAME} 
export AWS_INTRO_SAMPLE_S3_REGION={이미지 저장용 S3 리전}
export AWS_INTRO_SAMPLE_53_BUCKET={이미지 저장용 S3 버킷}
export AWS_INTRO_SAMPLE_REDIS_ADDRESS={일래스틱캐시 주소} 
export AWS_INTRO_SAMPLE_SMTP_DOMAIN={Amazon SES 도메인}
export AWS_INTRO_SAMPLE_SMTP_ADDRESS={Amazon SES 주소} 
export AWS_INTRO_SAMPLE_SMTP_USERNAME={SMTP 사용자}
export AWS_INTRO_SAMPLE_SMTP_PASSWORD={SMTP 비밀번호}
```

![[Pasted image 20240303180739.png]]


#### 파일 변경 반영하기
```BASH
source ~/.bash_profile
```


#### 테이블 생성
```BASH
rails db:migrate RAILS_ENV=production
```


#### 샘플 애플리케이션 실행
- 사용자로 전환
```BASH
exit
```


- 변경한 설정 갱신
```BASH
sudo systemctl restart nginx.service
```


- 샘플 애플리케이션 실행
```BASH
sudo su - deploy

cd /var/www/aws-intro-sample/

rails assets:precompile RAILS_ENV=production

rails server -e production
```


# 14. 서비스 모니터링하기  
#### 14장에서 만들고자 하는것은?

14.1 모니터링이란? 272  
14.1.1 집중 관리 272 / 14.1.2 경보 273 / 14.1.3 지속적인 정보 수집 274  
14.2 주요 모니터링 항목 274  
14.2.1 생사 모니터링 275 / 14.2.2 CPU 사용률 275  
14.2.3 메모리 사용률 276 / 14.2.4 디스크 용량 276  
14.2.5 네트워크 트래픽 276 / 14.2.6 리소스별 모니터링 항목 277  
14.3 CloudWatch 278  
14.4 리소스 모니터링하기 279  
14.4.1 모니터링 순서와 기능 279 / 14.4.2 대시보드 생성 281  
14.4.3 위젯 생성 283 / 14.4.4 경보 생성 288 / 14.4.5 미리 보기 및 생성 293  