# 3.안전한 조작 준비하기(IAM)
#### 3장에서 만들고자 하는 것은?
AWS에서 작업하기 위한 준비(**권한 설정 및 인증**)
![image](https://github.com/sprae114/Study/assets/52237184/507890f9-cdba-4894-89af-b89e25f567c8)

#### IAM 이란?
리소스 접근을 안전하게 관리하는 시스템으로, **인증과 접근허가** 기능을 구현함.

#### 인증과 접근 허가란?
- 인증 : 현재 이용하는 사용자가 **누구인지에 관한 정보**를 AWS에 전달하는 과정이다. 사용자에게는 고유 ID 제공함.
![image](https://github.com/sprae114/Study/assets/52237184/4e42df01-380b-4311-b7a9-981fee2fb06f)

- 접근 허가 : AWS 사용자가 **어떤 기능을 사용**할 수 있는가를 관리하고 허가하는 것이다. 
![image](https://github.com/sprae114/Study/assets/52237184/d651b8e0-6a34-44a3-a15e-9de2fed89869)


#### 루트 사용자와 일반 사용자란?
- 루트 사용자 : AWS의 로그인한 계정으로, 매우 강력한 권한가져서 **관리 용도**로만 사용함.
- 일반 사용자 : **개발에 사용**하는 사용자
![image](https://github.com/sprae114/Study/assets/52237184/db4be643-fcf3-4e6d-9db3-25cf67e7cf44)


#### 그룹이 필요한 이유는?
사용자가 늘어나게 되면 **개별적으로 사용자에게 권한 부여하기가 힘듦**. 그래서 그룹에 권한을 부여함.
![image](https://github.com/sprae114/Study/assets/52237184/25767470-cb38-4ff9-ba7c-c054b9aea080)


#### IAM 대시보드를 이용해 안전성 확인하기
추후에 다시 학습하기.

  
# 4.가상 네트워크 만들기  

#### 4장에서 만들고자 하는 것은?
관리자가 자유롭게 다양한 서버를 구축할 수 있는 네트워크 설정
![image](https://github.com/sprae114/Study/assets/52237184/39f0ff01-b162-4d6e-914f-b27ac0d1a8ba)


#### 네트워크란?
**인프라스트럭처(=인프라, 기반시설) 관리자가 주체가 되어 관리**하는 장소임. 네트워크 내의 기기는 LAN을 통해서 자유롭게 통신할 수 있음.
![image](https://github.com/sprae114/Study/assets/52237184/0a4f177e-632b-4703-bff4-976ec89eb8bc)


#### VPC이란?
AWS의 데이터센터에 있는 전용기기에서 서버나 네트워크 장비를 작동시켜, **클라우드 환경에서 제공되는 가상 사설 네트워크 서비스**이다. VPC끼리는 독립적으로 실행됌.
![image](https://github.com/sprae114/Study/assets/52237184/a66622b1-3aff-43ee-94cf-1538c1fc35cc)


#### VPC 설정 내용
![image](https://github.com/sprae114/Study/assets/52237184/3cc864e8-2aac-4e7f-99ea-975a969b9fd8)


#### 서브넷이란?
VPC의 **IP주소 범위를 나누는 단위**로, VPC안에는 하나 이상의 서브넷을 만들어야함.

주소 범위를 나누는 이유는
1) **역할 분리** : 외부에 공개하는 리소스 여부 구별(public/private)
2) **기기 분리** : AWS 안에서의 물리적인 이중화(다중화)를 수행함. AWS 내부의 서브넷에서 데이터와 인프라를 보호하기 위해 물리적으로 **분산 배치하는 전략**을 사용한다는 의미입니다. 이렇게 함으로써, 일부 컴포넌트의 장애가 발생해도 시스템 전체의 안정성과 지속성을 유지할 수 있습니다.


#### 가용영역이란?
클라우드 컴퓨팅에서 인프라스트럭처를 위한 **독립적인 데이터 센터를 의미**합니다.
![image](https://github.com/sprae114/Study/assets/52237184/5043d173-dbd1-4b77-83a0-84c69a4df22b)


#### IPv4 CIDR 설계 방법은?
CIDR 설계시 고려해야할 점
1) **생성할 서브넷의 수**
2) 서브넷 안에 생성할 **리소스 수**

![image](https://github.com/sprae114/Study/assets/52237184/ee5a0956-e335-48cd-bb98-68ce11954dc8)
![image](https://github.com/sprae114/Study/assets/52237184/d370a188-87a2-4ec8-a1a8-bc647e949f36)


#### 서브넷 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/54405164-174c-4907-abeb-c56bb3e35fc3)

![image](https://github.com/sprae114/Study/assets/52237184/49470de6-28a6-4482-af58-2ef7813d4772)


#### 인터넷 게이트웨이란?
VPC에서 생성된 **네트워크와 인터넷 사이의 통신을 가능**하게 하는 것. 인터넷 게이트웨이가 없다면 인터넷과 VPC 안의 리소스는 통신할 수 없음.
![image](https://github.com/sprae114/Study/assets/52237184/6bea6eb3-fcf5-4127-a922-df740bf33edb)


#### NAT 게이트웨이란?
**사설 IP 주소와 공인 IP 주소 간의 변환을 수행하는 네트워크 기술**입니다. 프라이빗 서브넷에 생성된 리소스는 인터넷으로 내보낼 수는 있지만 인터넷에 접근할 수 없어야함. 이를 위해 NAT 게이트웨이가 등장함. 퍼블릭 서브넷에 생성함.
![image](https://github.com/sprae114/Study/assets/52237184/92efbafa-30d0-44fa-a63d-a75d7c622819)

![image](https://github.com/sprae114/Study/assets/52237184/b0e17f34-c26b-468d-a6c2-01226fedcbe7)

![image](https://github.com/sprae114/Study/assets/52237184/3385094f-85bf-4249-9cfd-0f52e6cacf5d)


####  NAT 게이트웨이 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/882b7545-56ae-45af-951e-6532e16f1f5b)


#### 라우팅 테이블이란?
VPC안에 서브넷을 생성하고 리소스를 생성할 장소를 준비했음. (public/private/NAT gateway) 그리고 인터넷을 통신할 수 있는 출입구도 만들었음. (inter gateway) 그리고 이제는 **서브넷과 각각 게이트 웨이 또는 서브넷과 서브넷이 통신 할 수 있는 경로**를 만들어야함.
![image](https://github.com/sprae114/Study/assets/52237184/45fcd322-23f2-4a6b-b79a-ec13d48885a0)

**통신 경로를 설정하기 위해** 라우팅 테이블을 사용함.
![[Pasted image 20230705165300.png]]
- 송신 대상지 : 접속 대상의 위치
- 타깃 : 경우지
![image](https://github.com/sprae114/Study/assets/52237184/f5644843-11f9-4246-a30c-79e50f93e07b)


#### 라우팅 테이블 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/fc0a5a4c-22a2-424e-b885-8fd65226a48e)

![image](https://github.com/sprae114/Study/assets/52237184/d81af3bc-0311-411d-9a7a-b315ff63c60a)

![image](https://github.com/sprae114/Study/assets/52237184/4509e93a-f136-478f-a381-7c364317fb1d)


#### 보안 그룹이란?
인터넷을 통해 모든 리소스에 접근하는 문제점이 생김. **VPC 안의 리소스를 보호하기 위해 외부로부터 접근을 제한**하는 보안 그룹을 설정해야함.

![image](https://github.com/sprae114/Study/assets/52237184/32abae85-18bd-423a-b4ee-ddbe6c2dd007)


#### 보안 그룹이 제어하는 2가지 방법은?
1) 포트 번호
포트 번호로 서비스 종류 지정함. EX) HTTP(80번), HTTPS(443번), SSH(22번)

2) IP 주소
회사나 학교 조직 내에서 네트워크를 작업할 경우 IP주소를 한정할 수 있음. (IP 주소가 변하지 않으니깐)

#### 보안 그룹 생성내용
![[Pasted image 20230705193055.png]]


#### 보안그룹을 왜 두개나 설정할까?
- **점프서버** : 모든 리소스에 접속하는 입구로, 주로 관리자 접근용으로 사용되는 중간 서버임. 외부에서 직접적으로 접근하지 못하도록 막은 내부 인스턴스에 접근하기 위한 보안 채널을 제공함. **내부 인스턴스에 SSH 또는 RDP를 사용하여 접속하기 위해**서는 점프 서버를 거치도록 되어 있음.
- **로드 밸린서** : **요청이나 처리를 분산하는 역할**로, 사용자 트래픽을 여러 인스턴스로 분산하여 예기치 않은 장애 및 서버 부하를 줄임.


#### 네트워크 ACL과 보안 그룹 차이는?
- 공통점 : **접근 제한을 수행**하기 위한 시스템
- 보안 그룹 : **리소스**(EC2, 로드 밸린서, RDS 등)에 대해 설정 가능
- 네트워크 ACL : **네트워크에 대한 설정**. 즉, 해당 서브넷에 포함되는 리소스 모두에 적용

![image](https://github.com/sprae114/Study/assets/52237184/b2f19bd7-946b-4871-93d1-e819100a89f0)


# 5.점프 서버 준비하기  
#### 5장에서 만들고자 하는것은?
**네트워크를 안전하게 보호하면서 리소스를 생성할 수 있도록 점프 서버를 준비**해야함.
![image](https://github.com/sprae114/Study/assets/52237184/a2df43e2-0e61-40c2-bac1-cb9beed0ce1f)


#### 점프 서버란?
**모든 리소스에 접속할 수 있는 입구 역할**임. 리소스의 접속은 제한된 **관리자만 수행**할 수 있어야함. 그래서 모든 리소스에 접속할 수 있는 점프 서버를 준비하고, 해당 서버를 경유해야만 각 리소스에 접근할 수 있는 방식을 많이 사용함.

- 점프 서버는 EC2를 이용해서 구축하고 SSH를 이용해 접속함. 
- 리소스 통로 의외의 용도는 없음. 
- SSH을 경유할려면 비밀키와 공유 키라는 키페어를 준비해야함.
![image](https://github.com/sprae114/Study/assets/52237184/47ab267b-d850-4468-aabd-02622dbd13a7)


#### SSH 접속에 필요한 키 페어 준비하기
- 키는 작업자의 고유한 이름으로 저장하자.
- 윈도우 환경에서는 .pem 형식으로 저장하자
![image](https://github.com/sprae114/Study/assets/52237184/694b9de7-b0d3-423e-8280-0165006fc17f)


#### 점프 서버 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/b3aa986f-8d3b-452b-acb4-b9a70603f11a)

  
# 6.웹 서버 준비하기  
#### 6장에서 만들고자 하는것은?
웹을 제공하는 리소스 만들기
![image](https://github.com/sprae114/Study/assets/52237184/24e14ba3-4b86-4a6d-ab58-cb8cc04f8101)


#### 웹 서버란?
브라우저나 웹으로부터 **요청을 받아서 HTML이나 JSON 등의 응답을 반환하는 역할**.
![image](https://github.com/sprae114/Study/assets/52237184/803d82f5-7885-45a2-9398-c15edadd2515)


#### 웹 서버 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/aa1b67b9-eda9-4ca2-86a5-23ae721463ab)


#### 웹 서버 VS 점프 서버
▶점프서버
- **시스템 관리자**가 가끔 이용함
- **인터넷에 직접 연결됌**

▶웹서버
- 웹 **서비스 사용자**가 항상 연결을 시도함.
- **로드 밸린서를 통해 간접 연결**됌.
![image](https://github.com/sprae114/Study/assets/52237184/2ea4825c-d9a6-4fd9-8302-b8d247475c9c)


#### 기존 SSH 연결의 문제점은?
기존에는 EC2 인스턴스를 생성하고 SSH 명령을 통해서 연결을 확인했다. 하지만 연결할 것이 많아진다면 다음과 같은 문제점이 있다.
1) SSH 명령어를 N번 입력해야함.
2) 비밀 키 파일을 점프 서버에 전송해야함. (보안상 위험함)


#### 다단계로 접속 하기
점프 서버 1대와 웹 서버 2대 총 3대의 서버를 설정하는 파일을 만듦.
![image](https://github.com/sprae114/Study/assets/52237184/058a7777-0234-4188-bc4a-a830f61bab34)
- Host 항목 : 연결할 서버 이름
- Hostname 항목 : 연결할 서버의 IP주소 또는 서버 이름
- User 항목 : 연결할 때의 사용자 이름 지정함.
- IdentityFile 항목 : 비밀 키 파일의 경로를 지정함.
- ProxyCommand 항목 : 경우하는 점프 서버의 정보를 지정함. `ssh.exe {점프서버의 별명} -W %h:%p` 형식

이후, ssh 명령어 `ssh {서버별명}` 을 통해서 연결함.
![image](https://github.com/sprae114/Study/assets/52237184/fc8d7644-bdf3-49cf-a02a-bdf4d9e87452)

  
# 7.로드 밸런서 준비하기 113  
#### 7장에서 만들고자 하는것은?
웹서버는 아직 인터넷에 공개되지 않은 상태임. **로드 밸린서를 이용해 브라우저에서 애플리케이션을 열람**할 수 있도록 해보자.
![image](https://github.com/sprae114/Study/assets/52237184/97fb5013-8028-4c0f-9e51-4fa051441130)


#### 로드 밸런서란? 
사용자가 늘어나게 되면 1대의 웹 서버로는 요청을 처리할 수 없는 시점이 온다. 그래서 **웹 서버를 여러 대 준비하고 트래픽을 분산**할 수 있는 기술이 필요함. 


#### 로드 밸런서의 역할은?
![image](https://github.com/sprae114/Study/assets/52237184/97ff0d32-4534-4e10-bf82-4ded19cede5a)

1) **요청분산**
인터넷으로부터 전송된 요청을 여러 웹 서보에 균등하게 분산하는 것.
![image](https://github.com/sprae114/Study/assets/52237184/c91152f6-5b56-49f5-a9f7-f6bba77ccec0)

2) **SSL 처리**
송수신하는 데이터를 암호화/복호화하는 SSL처리.

3) **부정 요청 대응**
웹 서버에 올바른 요청이 아닌, 예상치 못한 작동을 일으키는 부정한 요청에 대응함.


#### AWS에서 제공하는 로드 밸런서 종류는?
▶Application Load Balancer(**ALB**)
**HTTP, HTTPS 이용한 접근을 분산**하는 데 최적화된 로드 밸린서임. SSL처리를 수행하거나 URL 패턴과 같은 분산 대상지를 바꾸는 등 고도의 기능을 제공함

▶Network Load Balancer(**NLB**)
기본적인 분산처리 기능만 제공하지만, **다양한 프로토콜**(EX 양방향 통신)에 대응하는 로드 밸린서임.
![image](https://github.com/sprae114/Study/assets/52237184/35f73e57-0550-4be1-9864-90e608c7c110)


#### 로드 밸런서를 이용한 요청 라우팅이란?
로드 밸런서가 외부에서 들어오는 클라이언트 요청을 받고, 이를 내부 웹 서버로 안전하게 전달하기 위해 **프로토콜과 포트 번호를 변환하는 역할을 수행**한다는 것을 의미합니다.

로드 밸런서는 클라이언트로부터 요청을 수신하고 이를 여러 백엔드 서버로 확산시키는 역할을 합니다. 이 과정에서 로드 밸런서가 다음과 같은 기능을 수행합니다:

1. 외부 클라이언트가 사용하는 특정 프로토콜(HTTP, HTTPS 등) 및 포트 번호(예: 80, 443)로 요청을 수신합니다.
2. 로드 밸런서는 요청을 내부 웹 서버로 전달하기 전에, 필요한 경우 프로토콜과 포트 번호를 변환합니다. 예를 들면, 클라이언트로부터 HTTPS를 통한 요청을 받은 로드 밸런서는 내부 웹 서버와 통신 시 HTTP를 사용할 수 있습니다.

이렇게 함으로써 로드 밸런서는 보안성을 유지한 상태에서 웹 서버에 요청을 안전하게 전달합니다. 요청의 프로토콜 및 포트 번호 변환을 통해 클라이언트와 내부 웹 서버 사이에 데이터를 효율적으로 전송하고, 웹 서비스의 성능과 가용성을 높입니다.


#### 로드 밸런서 생성 내용
![image](https://github.com/sprae114/Study/assets/52237184/00a4c166-924e-48b0-9812-2186f6de804d)

![image](https://github.com/sprae114/Study/assets/52237184/de2b9fd8-30bc-43b1-a36e-b872b2db9ebe)


#### 가용 영역 선택하기
로드 밸린서가 이용하는 가용 영역 선택하기. 선택할 수 있는 곳은 서브넷을 생성한 가용영역 뿐임.
![image](https://github.com/sprae114/Study/assets/52237184/4f98f375-f9b5-4a97-a38b-a5037ea2a426)


#### 로드 밸런서와 대상 그룹 설정하기
ALB을 사용할때 설정해야할 두가지 목록이 있음.
1) **로드 밸런서** : 어떤 프로토콜(HTTP, HTTPS)을 이용할 것인지와 같은 로드 밸런서로 접근에 관련한 설정
2) **대상그룹** : 어떤 웹 서버에 요청을 분산할 것인가와 같은 웹 서버에 접근에 관련 설정
![image](https://github.com/sprae114/Study/assets/52237184/372ce184-b300-4296-b1fa-d947c8c0b9c8)


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
## 모니터링 3가지
#### 집중 관리 
![[Pasted image 20240303073940.png]]
모든 서비스의 정보를 한 곳에서 집중 관리 할수 있는 장소를 대시보드라고 한다.


#### 경보 
![[Pasted image 20240303073953.png]]
서버가 장애를 일으키거나 이용자가 갑자기 증가해 반응이 지연되는 등 무언가 대응이 필요한 경우에 이를 감지할 시스템이 필요하다.


#### 지속적인 정보 수집
![[Pasted image 20240303074006.png]]
해당 문제의 원인이 발생 시점뿐만 아니라 평소에도 자주 나타났을 수 있다. 이를 위해 정보를 지속적으로 저장해야함.


## 주요 모니터링 항목
#### 생사 모니터링 
![[Pasted image 20240303074358.png]]

#### CPU 사용
![[Pasted image 20240303074410.png]]

#### 메모리 사용률 
![[Pasted image 20240303074427.png]]

#### 디스크 용량
![[Pasted image 20240303074434.png]]

#### 네트워크 트래픽 
![[Pasted image 20240303074443.png]]


## Cloud Watch 사용방법은?
[(15) AWS의 눈 - Cloud Watch - YouTube](https://www.youtube.com/watch?v=jGryI-hBA38&list=RDCMUCvc8kv-i5fvFTJBFAk6n1SA&start_radio=1&rv=jGryI-hBA38&t=73&ab_channel=%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9)
