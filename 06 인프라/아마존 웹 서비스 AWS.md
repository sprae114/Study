# 1장 클라우드와 아마존 웹 서비스
## 클라우드 컴퓨팅
#### 📌 클라우딩 컴퓨팅이란?
인터넷 통신 서비스를 활용해서 연결된 원격 컴퓨터를 활용하는 기술	
![[Pasted image 20221120235744.png]]
- [00:15](https://www.youtube.com/watch?v=IH7mUwunzlo&t=32s#t=15.262537240325928) : <얄팍한 코딩사전> 클라우딩 컴퓨팅 설명
건물 하나를 빌리는 온프레미스 방식과 가성비 좋은 초대형 호텔을 빌리는 클라우드 방식이 있음.

#### 📌 클라우딩 컴퓨팅 서비스 이용방식
![[Pasted image 20221123011833.png]]
![[Pasted image 20221123011904.png]]

- [03:26](https://www.youtube.com/watch?v=IH7mUwunzlo&t=32s#t=206.9138160371933) :<얄팍한 코딩사전> 서비스 이용 설명
![[Pasted image 20221124163320.png]]
IaaS : 조립형 컴퓨터를 사서 원도우 깔고 드라이버 다운받고 운영관리 한다고 생각하자
PaaS : 내가 짠 코드만 압축해서 올리면 클라우드에서 알아서 서버에 넣고 돌려줌
SaaS : 만들어진 소프트웨어 (이메일, 유튜브)같은 서비스

#### 📌 장점
1) 초기비용 발생 X, 사용한 만큼 지불하면 됌
2) 미래에 필요한 인프라 걱정없이 확장을 쉽게 할 수 있음
3) 데이터 센터 운영 및 유지 관리에 비용 투자가 필요하지 X
4) 몇분만에 전세계로 서비스 런칭 가능

## AWS 서비스
#### 1) 컴퓨팅 서비스
![[Pasted image 20221123145209.png]]

#### 2) 네트워킹 서비스
![[Pasted image 20221123145130.png]]
![[Pasted image 20221123145153.png]]

#### 3) 스토리지 서비스
![[Pasted image 20221123145116.png]]

#### 4) 데이터베이스 서비스
![[Pasted image 20221123145104.png]]

#### 5) 분석 플랫폼
![[Pasted image 20221123145049.png]]

#### 6) 애플리케이션 서비스
![[Pasted image 20221123145038.png]]

추가 :  [[AWS] 전체서비스 : 본 내용은 AWS 서비스에 어떤것들이 있는지](https://medium.com/harrythegreat/aws-%EC%95%84%EB%A7%88%EC%A1%B4%EC%9B%B9%EC%84%9C%EB%B9%84%EC%8A%A4-overview-1-4cc3ffdd6b59)

#### ❓스토리지와 데이터베이스의 차이점은?
 - 스토리지는 파일형태가 되면 무엇이든 담을 수 있지만 DB에 담기 위해서는 앞단 또는 뒷단에서 가공해서 저장
출처 : [[DB/Storage] Database와 Storage의 차이점 (tistory.com)](https://skstp35.tistory.com/306)

# 2장 확장성과 안정성 높은 서버 만들기

## 용어정리
#### 📌 서버
- **특화된 어떤 임무를 수행**하기 위해 설계된 컴퓨터
- 대용량의 서비스를 빠르게 처리하기 위해 높은 사양의 하드웨어 요구
- AWS는 **EC2서비스**를 통해 가상의 서버를 구성
	why? 가상의 서버를 구성할까?
	물리적 컴퓨터 한대에 가상의 컴퓨터를 여러개 띄울 수 있어 자원을 효율적으로 쓸수 있음.

#### 📌 하드디스크
- **데이터를 저장, 검색, 삭제를 수행하여, 영구적으로 저장**하기 위해 사용되는 컴퓨터 장치
- 대량의 데이터 처리를 위해 디스크 어레이컨트롤러와 RAID 기술 사용함
- AWS는 **EBS 서비스**를 이용하여 EC2에 디스크 추가할 수 있음
추가 : [RAID란? RAID의 종류와 구성방식](https://jwprogramming.tistory.com/24)

#### 📌 보안
- 정보 및 데이터를 **안전한 상태로 유지**하는 것
- AWS는 **IAM**(리소스 암호화 서비스), Shield, WAF등이 있음

#### 📌 방화벽
- 내부 네트워크의 중요한 정보 자산에 대해 **외부로부터 불법적인 침입을 보호**하기 위한 시스템
- AWS는 **Security Group, NACL, WAF**을 통해 방화벽 서비스 제공

#### 📌 리전
- 가장 가까운 곳에서 클라우드 서비스를 이용할 수 있도록 전 세계의 각 **데이터 센터를 묶는 물리적 위치**

#### 📌 가용영역(AZ)
- **데이터 센터**
- 천재지변으로 부터 데이터를 보호하기 위해 가용 영역이 떨어져 있어야함.

#### 📌 CDN
- 서버와 물리적으로 사용자들이 빠르게 받을 수 있도록 전세계 곳곳에 위치한 **캐시 서버에 복제해주는 서비스**

#### 📌 엣지 로케이션
- CDN 서비스를 위해 **캐시 서버의 모음**을 뜻함.

#### 📌 EC2 (Elastic Compute Cloud)
- 서비스 설명
![[Pasted image 20221123145305.png]]

- 인스턴스 구매 옵션
![[Pasted image 20221123020902.png]]

#### 📌 EBS (Elastic Block Storage)
- 서비스 설명
![[Pasted image 20221123145246.png]]

- 봄륨 유형
![[Pasted image 20221123021125.png]]

#### 📌 스냅샵 활용
- EBS 볼륨의 데이터를 스냅샷으로 만들어 Amazon S3에 백업 및 보관할 수 있는 기능 (**하드디스크 통쨰로 백업**한다고 생각하자)

## 보안그룹 (Security Group)
#### 📌 보안그룹이란?
인스턴스에 대한 인바운드, 아웃바운드의 **네트워크 트래픽을 제어**하는 가상의 방화벽 역할을 수행

#### 📌 주요 특징
1) 네트워크 트래픽을 위해 "허용" 정책은 있으나, "차단" 정책은 없음
2) 인바운드 트래픽과 아웃바운드 트래픽을 별도로 제어 가능

📌 EC2와 EBS를 이용한 실습 (P. 57)
📌 보안그룹 강화하기 실습(P. 74)

# 3장 무한대로 저장 가능한 스토리지 만들기

## 용어정리
#### 📌 스토리지
- 컴퓨터의 **하드디스크와 동일한 역할**
- 스토리지 서비스는 **S3(데이터를 무한하게 저장 가능함)**, **Glacier(대용량의 데이터를 백업 및 보관 이 가능함)**

#### 📌 스토리지 연결방법
![[Pasted image 20221123024438.png]]
a) DAS
- 정의 : **시스템을 서버에 직접 부착하는 방식**으로 연결되어 있는 클라이언트(컴퓨터)를 이용해야만 스토리지 시스템에 저장되어 있는 데이터에 액세스할 수 있음.
- 장점 : 소규모 스토리지 시스템을 구성할 때 비용이 경제적
- 단점 : 공유가 어렵고 여러 서버로 구성된 분산 환경에서는 관리가 어렵다.
b) NAS와 SAN
- 연결방식 : 스토리지를 **빠른 속도의 네트워크로 연결하는 방식**
- 차이
	NAS : **LAN을 이용**해 연결하여 가격 저렴함. **블록 수준에서 데이터를 저장함.**
	SAN : **고속의 전용 네트워크로 구성. 파일 단위로 데이터에 접속함**
추가자료 :  [NAS와 SAN Storage의 차이? (tistory.com)](https://sunrise-min.tistory.com/entry/NAS%EC%99%80-SAN-Storage%EC%9D%98-%EC%B0%A8%EC%9D%B4)

#### 📌 데이터 백업
- 데이터 손실을 대비하여 복사하여 다른 곳에 저장하는 것
- 데이터 손실이 일어나는 경우는? ![[Pasted image 20221123151000.png]]
- AWS에서는 **EBS의 Snapshot, AMI백업** 기능 제공

#### 📌 스냅샷
- **특정 시간에 데이터 저장 장치의 상태를 별도의 파일이나 이미지로 저장**하는 기술.
- 스냅샷 예시![[Pasted image 20221123151257.png]]
- 데이터를 보호 + 대용량 데이터의 백업 관리를 단순화하여 운영 비용 최소화함.

#### 📌온프레미스(On-premise) vs 오프프레미스(Off-premise)
- 온프레미스 : **직접 인프라를 구축**하는 방식
- 오프프레미스 : **클라우드 서비스**를 이용하는 방식

#### 📌 S3 (Simple Storage Service)
- 무한대로 저장 가능하고, 사용한 만큼 지불하는 **인터넷 기반 스토리지**
- **버킷**이라는 리전내에서 유일한 영역을 생성하고 **데이터를 키-값 형식의 객체로 저장**함.
- 서비스 설명![[Pasted image 20221123151441.png]]
- 스토리지 클래스 옵션![[Pasted image 20221123152037.png]]
#### 📌 버킷이란?
 S3에 저장된 객체에 대한 **컨테이너**
출처 : [버킷 개요 - Amazon Simple Storage Service](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/UsingBucket.html)

#### 📌 Glacier
- Glacier이란?
**데이터 아카이빙** 및 장기 백업을 위한 안전하고 안정적이며 비용이 매우 저렴한 **클라우드 스토리지 서비스**

- 데이터 아카이빙이란?
현재 운영 시스템 및 데이터베이스에서 **사용 빈도가 낮은 데이터를 확인하여 장기간 저장 가능한 스토리지 시스템으로 옮기는 프로세스**
출처 : [아카이빙이란 - 아카이빙 솔루션 | Dell Korea](https://www.dell.com/ko-kr/dt/learn/data-protection/what-is-archiving.htm)

- 전송방법
![[Pasted image 20221123152642.png]]

- 서비스 설명
![[Pasted image 20221123152406.png]]

- 데이터 접근 방법
1) API/SDK를 이용한 Direct연결로 프로그램 개발을 통해 직접 접속함
2) S3 라이프 사이클 통합을 통해 오래된 데이터 자동으로 Glacier로 이관
3) 3rd Patty Tool과 Storage Gateway 연동을 통해 데이터 백업 및 보관기능

#### 📌 AMI (Amazon Machine Image)
- EC2 인스턴스 **생성에 필요한 모든 소프트웨어 정보**를 담고있는 템플릿 이미지
- 동일한 환경을 갖는 인스턴스를 생성가능함

📌 실습 : 무한대로 저장 가능한 S3로 파일 업로드 및 삭제하기(P.91)
📌 실습 :  AMI를 이용한 서버 백업과 복원하기(P.104)
📌 실습 : S3 버킷 삭제하기(P.109)

# 4장 독립적인 나만의 가상 네트워크 공간 만들기
## 사전지식
### 우테코
- [02:29](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=149.07889695708465) : VPC
![[Pasted image 20221123163851.png]]

- [02:56](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=176.05954234618378) : Subnet
![[Pasted image 20221123163913.png]]

- [03:06](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=186.85011711730195) : 인터넷 트래픽이 EC2에 도달하는 과정
![[Pasted image 20221123164030.png]]

- [04:56](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=296.06287911444093) : NACL과 보안그룹의 차이는?
![[Pasted image 20221123164133.png]]

- [05:20](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=320.8134960200272) : VPC, 서브넷에 IP를 할당하는 방법
![[Pasted image 20221123164236.png]]

- [06:06](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=366.21063681403353) : ACL, 보안그룹 설명
![[Pasted image 20221123164335.png]]

- [07:05](https://www.youtube.com/watch?v=tkP5u_SrF-8&t#t=425.115948123024) : NAT Gateway
Private Subnet은 외부에서 접근하지 못하는 데이터베이스로 사용함. 그래서 인터넷이 연결 안되서 여러 설치파일을 다운받지 못하는 문제점 생김.
NAT Gatway은 **Publice 다운받고 private로 전달해주는 역할**을함
![[Pasted image 20221123165449.png]]

### 가장쉽게 VPC 개념잡기
#### 📌 VPN
![[Pasted image 20221123173218.png]]
그림과같이 회사의 네트워크가 구성되어있고 보안상의 이유로 직원간 물리적으로 네트워크를 분리하고싶다면 기존 인터넷선 선공사도 다시해야함.

![[Pasted image 20221123173325.png]]
하지만 VPN은 네트워크A와 네트워크B가 실제로 같은 네트워크상에 있지만 논리적으로 다른네트워크인것처럼 동작함.

#### 📌 VPC
![[Pasted image 20221123173403.png]]
VPC가 없다면 EC2 인스턴스들이 서로 거미줄처럼 연결되고 인터넷과 연결됌. 이렇게 되면 인스턴스 변경에 불편함 생김.

![[Pasted image 20221123173527.png]]
VPC를 적용하면 위 그림과같이 VPC별로 네트워크를 구성할 수 있고 각각의 VPC에따라 다르게 네트워크 설정을 줄 수 있습니다.

#### 📌 VPC를 구축하는 과정
1) VPC의 아이피범위를 RFC1918이라는 **Private IP대역에 맞추어 구축**해야함. (Private IP 할당)
- Private IP이란? 인터넷을 위해 사용하는것이 아닌 우리끼리 사용하는 아이피주소 대역
![[Pasted image 20221123173746.png]]

2) 서브넷 만들기
- 서브넷이란? **VPC를 잘개 쪼개는 과정**을 통해 더 많은 네트워크망을 만들기 위함.
![[Pasted image 20221123174211.png]]
❓서브넷을 사용하는 이유는 뭐지?
**도메인의 크기를 줄이기** 위함. 만약 단일 네트워크에 부여했다면, 과다한 호스트 컴퓨터의 수로 인해 네트워크 트래픽이 증가할 것이다.

3) 라우팅 테이블과 라우터
- 네트워크 요청이 발생하면 데이터는 우선 라우터로 향하게됩니다. 
- 라우터란 목적지이고 라우팅테이블은 각 목적지에 대한 이정표
- 데이터는 라우터로 향하게되며 네트워크 요청은 각각 정의된 라우팅테이블에따라 작동함.
- if) 범위 밖에 네트워크 요청이 온다면, 처리할수 없음
![[Pasted image 20221123175726.png]]

❓ 라우터란
- 패킷을 목적지까지 전달하기 위해 다음 네트워크 지점을 결정하는 장치나 컴퓨터 내의 소프트웨어
출처 :[[ Network ] 라우터( Router )란 (tistory.com)](https://chunggaeguri.tistory.com/entry/Network-%EB%9D%BC%EC%9A%B0%ED%84%B0-Router-%EB%9E%80)


4) 인터넷게이트웨이
- 인터넷게이트웨이란? VPC와 인터넷을 연결해주는 하나의 관문
- 서브넷B의 라우팅테이블을 잘보면 0.0.0.0/0으로 정의되어있습니다. 이뜻은 모든 트래픽에 대하여 IGA(인터넷 게이트웨이) A로 향하라는뜻
![[Pasted image 20221123180246.png]]

5) 네트워크 ACL과 보안그룹
- 네트워크 ACL과 보안그룹은 방화벽과 같은 역할을 하며 인바운드 트래픽과 아웃바운드 트래픽 보안정책을 설정
- 보안그룹이란? Stateful한 방식으로 동작하는 보안그룹은 모든 허용을 차단하도록 기본설정 되어있으며 필요한 설정은 허용
-  네트워크ACL이란? 각각의 보안그룹별로도 별도의 트래픽을 설정할 수 있으며 서브넷에도 설정할 수 있지만 각각의 EC2인스턴스에도 적용.
![[Pasted image 20221123180401.png]]

6) NAT 게이트웨이
- NAT 게이트웨이란? 프라이빗서브넷이 인터넷과 통신하기위한 아웃바운드 인스턴스
- 프라이빗 네트워크가 외부에서 요청되는 인바운드는 필요없더라도 인스턴스의 펌웨어나 혹은 주기적인 업데이트가 필요하여 아웃바운드 트래픽만 허용되야할 경우사용함.
![[Pasted image 20221123183143.png]]


출처 : [[AWS] 가장쉽게 VPC 개념잡기. 가장쉽게 VPC 알아보기 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-%EA%B0%80%EC%9E%A5%EC%89%BD%EA%B2%8C-vpc-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-71eef95a7098)

## 용어정리
#### 📌 Network
- 네트워킹한다 = 서로 통신을 한다

- 프로토콜이란?
통신을 위해 지켜야하는 약속

#### 📌 VPN(Virtual Private Network)
- 컴퓨터을 연결하는 사설 네트워크를 만들거나, 원격으로 네트워크를 연결하고 암호화 기술을 적용하여 안정적이고, 보완성 높은 통신 서비스를 제공하는 서비스
![[Pasted image 20221123160154.png]]
- VPC와 VPC Gateway를 통해 On-Premise의 VPN장비와 VPN을 연결할 수 있음.


## VPC(Virtual Private Cloud)
#### 📌VPC란?
클라우드에서 논리적으로 격리된 네트워크 공간을 할당하여 가상 네트워크에서 AWS 리소스를 이용할 수 있는 서비스를 제공함.
![[Pasted image 20221123162348.png]]
![[Pasted image 20221123165959.png]]

#### 📌 Private IP, Publice IP, Elastic IP
- Private IP : 인터넷을 통해 연결할 수 없는, VPC 내부에서만 사용하는 주소
- Publice IP : 인턴스와 인터넷 통신을 위해 사용. 인스턴스가 재부팅되면 새로운 Publice IP 할당함.
- Elastic IP : 동적 컴퓨팅을 위해 Elastic IP사용함. 그래서 다른 인스턴스 주소를 매칭하여 인스턴스 장애 조치를 할수 있음.

#### 📌 VPC와 서브넷
- 가장쉽게 VPC 개념잡기로 이해하기

#### 📌 서브넷 사이즈
- CIDR이란? VPC에서 사용하게 될 IP 주소의 지정하는 범위

추가 : [[NW] 🌐 CIDR 이 무얼 말하는거야? ⇛ 개념 정리 & 계산법 (tistory.com)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-CIDR-%EC%9D%B4-%EB%AC%B4%EC%96%BC-%EB%A7%90%ED%95%98%EB%8A%94%EA%B1%B0%EC%95%BC-%E2%87%9B-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC-%EA%B3%84%EC%82%B0%EB%B2%95)

#### 📌 Public 서브넷과 Private 서브넷
- Public 서브넷 
네트워크 트래픽이 인터넷 게이트 웨이로 라우팅 되는 서브넷
인터넷망을 통해 서비스를 수행하는 웹 서버를 생성함
- Private 서브넷
네트워크 트래픽이 인터넷 게이트 웨이로 라우팅 되지 않는 서브넷
높은 보안성을 필요로 하는 DB서버를 생성함

#### 📌라우팅테이블
- 아웃바운드(서브넷 외부로 나가는 것) 트래픽에 대해 허용된 경로를 지정로 나침반과 비슷한 역할

## 주요 서비스
#### 📌 보안 그룹과 네트워크 액세스 제어(ACL) 목록
- 왜 사용하는 거지? 네트워크 통신과 트래픽에 대해 IP와 Port를 기준으로 통신을 허용하거나 차단하기 위한 기능을 제공
![[Pasted image 20221123210936.png]]

#### 📌 VPC 피어링 연결
- 피어링 연결이란? 비공개적으로 두 VPC 간에 트래픽을 라우팅할 수 있게 하기 위한 서로 다른 VPC 간의 네트워크 연결을 제공
![[Pasted image 20221123211121.png]]

#### 📌NAT(Network Address Translation) 게이트웨이
- NAT 게이트웨이란? 외부 네트워크에 알려진 것과 다른 IP주소를 사용하는 내부 네트워크에서, 내부 IP 주소를 외부 IP 주소로 변환하는 작업을 수행하는 서비스
- 보안상 중요한 서비이지만 윈도우 패치나 보안 업데이트, 소프트웨어 업데이트를 인터넷을 통해 받아야 하는 경우 사용함.
![[Pasted image 20221123211554.png]]

#### 📌 VPC Endpoint
- Private 서버는 인터넷과 연결되어 있는 S3와 같은 공용 리소스를 연결할 수 없음
- 그래서 NAT 게이트 웨이를 썻지만, VPC Endpoint를 이용하면, S3와 DynamoDB에 쉽고 빠르게 연결 가능
![[Pasted image 20221123211851.png]]

#### 📌 VPN(Virtual Private Network) 연결
- VPC에서 서비스되는 인스턴스는 On-Premise에 있는 서버나 IDC 내의 시스템과 통신할 순 없음.
- 그래서 VPC에 가상 프라이빗 게이트웨이를 연결하고, 라우팅 테이블 생성하며, 보안 그룹의 규칙을 업데이트해서 접속 가능하게하는 하이브리드 클라우드환경을 구성할 수 있음.
![[Pasted image 20221123212335.png]]

📌실습 : VPC 마법사를 통해 퍼블릭 서브넷과 프라이빗 서브넷 만들기(P.123)
📌실습 : 리전 간 VPC Peering으로 글로벌 통합 네트워크 환경 구축하기(P.124)

# 5장 확장 가능한 데이터베이스 서버 만들기
## 용어정리
#### 📌 데이터베이스
- 데이터베이스란? 여러 사람에 의해 공유되어 사용될 목적으로 통합하여 관리되는 데이터의 집합

#### 📌 RDBMS
- 연관 관계가 있는 데이터 항목들의 모음으로 데이터 테이블로 이루어져 있음.

#### 📌 RDS(Relational Database Services)
- 클라우드에서 관계형 데이터베이스를 더욱 간편한게 설정, 운영 및 확장할 수 있는 서비스
- 서비스 설명
![[Pasted image 20221123214233.png]]
- 서비스 이용 방법
![[Pasted image 20221123214345.png]]
![[Pasted image 20221123214449.png]]

- 주요특징
1) 유연한 인스턴스 및 스토리지 확장
2) 손쉬운 백업과 복원 기능
3) 멀티 AZ를 통한 고가용성확보
고가용성이란? 오랜 시간 지속하여 사용할 수 있는 시스템
4) RDS 암호화 옵션을 통한 보안성 강화
5) 데이터 마이그레이션 서비스
데이터베이스에 대한 데이터 이전을 지원

📌 실습 : MySQL용 DB 인스턴스 생성, 클라이언트를 통한 DB연결 및 삭제(P.142)
📌 실습 : 웹 서버에서 실행되는 PHP 애플리케이션에 MySQL 데이터베이스 연결하기(P.152)
📌 실습 : EC2/RDS 중지하기 (P.172)

# 6장 DNS 손쉽게 연결하고 관리하기

## DNS
#### 📌 DNS란? 
- 사람이 읽을 수 있는 도메인 이름(예 naver.com)을 다양한 디바이스에서 읽을 수 있는 IP주소(예 192.0.2.44)로 변환하는 역할.
- AWS에서는 Route35로 서비스 제공.

📌 DNS의 동작 원리
- 책 설명은 어려워...
![[Pasted image 20221123222625.png]]
![[Pasted image 20221123222633.png]]![[Pasted image 20221123222652.png]]

#### 📌 우테코 DNS 설명

- [00:37](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=37.566425959945676) : DNS를 사용하는 이유
도메인을 입력하는데 IP주소는 어떻게 알까?
![[Pasted image 20221123224239.png]]

- [01:07](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=67.474655) : DNS설명
![[Pasted image 20221123224347.png]]

- [01:42](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=102.5436451745224) : 간단한 DNS 동작과정
1) 브라우저 캐시에서 확인
2) 컴퓨터에 저장되어 있는 host파일과 캐시 확인
3) DNS 서버에 요청하기
![[Pasted image 20221123224510.png]]

- [02:20](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=140.49945595136262) : DNS 서버 분리하기
![[Pasted image 20221123224802.png]]

- [02:39](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=159.73528581403352) : DNS 계층 구조
![[Pasted image 20221123224859.png]]
![[Pasted image 20221124172347.png]]

- [03:52](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=232.2070460600815) : 자세한 DNS 동작과정 설명
![[Pasted image 20221124172738.png]]

- [05:25](https://www.youtube.com/watch?v=sDXcLyrn6gU#t=325.44861209441376) : 실제로 도메인 적용해보기

출처 : [[10분 테코톡] 엘리의 DNS - YouTube](https://www.youtube.com/watch?v=sDXcLyrn6gU)

#### 📌Route 53
- 서비스 설명
![[Pasted image 20221123231717.png]]

#### 📌 Route 53 주요특징
- 연결체크 및 Failover (Health Check and Failover)
![[Pasted image 20221124001242.png]]
연결된 응용프로그램의 각 끝점을 모니터링하여 서비스의 사용여부 확인후, 상태 반환.

- 고가용성 DNS 서비스 및 DNS Failover
![[Pasted image 20221124001353.png]]
상태 검사와 연결된 장애 조치 레코드를 구성함.

- 지연시간 기반 라우팅
![[Pasted image 20221124001933.png]]
지연시간이 가장 적은 리소스로 Route53에서 DNS 쿼리에 응답 처리함.

- 가중치 기반 라우팅
![[Pasted image 20221124002100.png]]
여러 리소스 레코드를 단일 DNS 이름으로 연결한 후 같은 기능을 수행하는 여러 리소스에 대해 사용자가 지정한 가중치 비율로 트래픽을 라우팅할 수 있음.

- 지역 기반 라우팅
![[Pasted image 20221124002321.png]]
지리적 위치를 기반으로 특정 엔드포인트에 대한 라우팅 수행

📌실습 : Amazon Route 53에서 DNS 구입하기 (P.184)
📌실습 : 다른 곳에서 구매한 도메인을 Amazon Route 53에 등록하기(P.188)
📌실습 : Route 53를 통해 웹 서버에 도메인 연결하기(P.191)

# 7장 네트워크 트래픽을 분산시켜 주는 로드 밸런싱

## 로드 밸런싱
#### 📌로드 밸런싱이란?
- 네트워크 기술의 일종으로 네트워크 트래픽을 하나 이상의 서버나 장비로 분산하기 위해 사용되는 기술
![[Pasted image 20221124002916.png]]

#### 📌로드 밸런싱의 방식
- Round Robin
real 서버로의 session 연결을 순차적으로 맺어주는 방식으로 Session에 대한 보장X.
- Hash
Client와 Server 간에 연결된 Session을 계속 연결해주는 방식으로 동일 서버로만 연결되는 구조로 Session보장함.
- Least Connection
Session 수를 고려하여 가장 작은 Session을 보유한 서버로 연결하는 방식으로  Session에 대한 보장X.
- Response Time
응답시간을 고려하여 빠른 응답시간을 제공하는 서버로 연결하는 방식으로  Session에 대한 보장X.

#### 📌 ELB(Elastic Load Balancing)
- Elastic 로드밸런싱이란? 가용영역에서 EC2 인스턴스 및 컨테이너, IP주소 같은 동일한 서비스를 제공하기 위해 여러 네트워크 트래픽을 자동으로 분산시킴
![[Pasted image 20221124021622.png]]

#### 📌ELB(Elastic Load Balancing)의 종류
![[Pasted image 20221124021817.png]]
![[Pasted image 20221124021851.png]]

#### 📌ELB의 주요 특징
1) 상태 확인 서비스 (Health Check)
- ELB와 연결된 인스턴스의 연결 상태를 수시로 체크하여 연결장애나 서비스 가능여부를 지속적으로 체크하는 것
![[Pasted image 20221124022117.png]]
2) Sticky Session
- 기본적으로 Round Robin 방식으로 트래픽을 분산함. 그러면, 세션 유지가 필요한 인증 정보를 유지할 수 없는 문제점이 생김.
- Sticky Session을 사용하면 Client에  별도의 HTTP 기반의 쿠키 값을 생성하여 다음번 연결 요청에 대해 처음 접속했던 서버로 계속 연결하도록 트래픽 처리함.
![[Pasted image 20221124022425.png]]

3) 고가용성 구성
- 여러 가용 영역에 있는 여러 대상(인스턴스, 컨테이너)에 걸쳐 트래픽을 자동으로 분산할 수 있음.
![[Pasted image 20221124022640.png]]

4) SSL Termination 및 보안기능
- SSL이란? 사용자의 웹브라우저와 인터넷 사이트의 웹서버간 암호화 통신을 가능하게 하는 제3의 신뢰기관이 인증한 인증서
출처 : [SSL 인증서의 모든 것(정의 및 기능, 신뢰성, 구성항목) (tistory.com)](https://yoonsj.tistory.com/14)

- 인증서를 직접 적용하려면, 인증서의 만료에 따른 갱신 등의 관리와 암호화 및 복호화 처리를 위한 추가 부하가 발생함.
-  ELB에 공인인증서 또는 ACM에서 무료 사설 인증서를 발급해서 해결함.
![[Pasted image 20221124023736.png]]

📌 실습 : 웹 구성 및 웹 페이지 연결 테스트(P.205)
📌 실습 : ELB 구성하기(P.211)
📌 실습 : 서비스 실패시 ELB 테스트하기(P.214)
📌 실습 : ELB 세션 연결 고정(Sticky Session활성화)하기(P.216)
📌 실습 : EC2/ELB 삭제하기 (P.220)

# 8장 가용성 높고 빠르게 확장 가능한 인프라 구성하기

## 용어정리
#### 📌가용성
- 가용성이란? 해당 시스텀이나 서비스가 가동 및 실행되는 시간의 비율
![[Pasted image 20221124024308.png]]

#### 📌확장성
- 확장성이란? 서비스나 응용프로그램이 증가하는 성능 요구에 맞춰 향상될 수 있는 정도
- 물리적 하드웨어 환경에서는 스케일업과 스케일 아웃 전략이 있음

#### 📌Amazon Auto Scaling
- Amazon Auto Scaling이란? 서버를 모니터링하고 리소스를 자동으로 조정하여, 안정적이고 예측 가능한 성능으로 유지하는 것
![[Pasted image 20221124024923.png]]
- 인스턴스의 조정 및 관리 목적으로 구성된 논리적 그룹으로 Auto Scaling을 수행하는 인스턴스의 모음
![[Pasted image 20221124034201.png]]
- 시작 구성을 생성하는 경우에는 AMI, 인스턴스 유형, 키 페어, 하나 이상의 보안그룹, EBS 등 인스턴스에 대한 정보가 필요함.

📌 실습 : ELB설정하기 (P.229)
📌 실습 : Auto Scaling 구성하기 (P.232)
📌 실습 : ELB/Auto Scaling 삭제 (P.244)

# 9장 CDN 서비스로 웹 사이트의 속도를 더욱 빠르게 하기
## CDN (Content Delivery Network)
#### 📌CDN이란?
- ISP의 CDN 서버에 콘텐츠를 분산시키고 유저의 가장 가까운 서버로 부터 콘텐츠를 전송받도록 하여 트래픽이 특정 서버에 집중되지 않고 각 지역 서버로 분산하는 기술

📌추가 : [CDN이 뭔가요? | 얄코 (yalco.kr)](https://www.yalco.kr/46_cdn/)
#### 📌CDN의 동작원리
![[Pasted image 20221124042617.png]]
1) 웹 브라우저가 실행되는 디바이스인 PC나 모바일 기기의 사용자 에이전트가 특정 주소에 접근하여 HTML, 이미지, CSS, JavaScript 파일 등 렌더링하는 데 필요한 콘텐츠를 서버로부터 요청
2) DNS는 콘텐츠에 대한 각 요청이 발생하면 End User와 가장 가까운 위치에 최적으로 배치된 CDN 서버에 End User가 매핑되고, 해당 서버는 요청된 파일의 캐싱된(사전 저장된) 버전으로 응답(전송). 
3) 서버가 파일을 찾는 데 실패하는 경우 CDN 플랫폼의 다른 서버에서 콘텐츠를 찾은 다음 End User에게 응답을 전송. 콘텐츠를 사용할 수 없거나 콘텐츠가 오래된 경우, CDN은 오리진 서버에 대한 요청 프록시로 작동하여 향후 요청에 대해 응답할 수 있도록 Patch된 새로운 콘텐츠를 저장
출처 : [CDN 동작 원리 :: 한량 개발자 (tistory.com)](https://ijbgo.tistory.com/32)

#### 📌CDN 캐싱 방식의 종류
- Static Caching
사용자의 요청이 없어도 Origin Server에 있는 Contents를 운영자가 미리 Cache Services에 복사함으로써 사용자가 Cache 서버에 접속하여 Contents를 요청하면 Cache서버가 콘턴츠를 전달하는 방식
![[Pasted image 20221124141406.png]]
- Dynamic Cahching
최초에는 Cache 서버에 콘텐츠가 없으나, 사용자가 콘텐츠를 요청하면 Cache서버에 콘텐츠가 있는지 여부를 확인함. 없으면 오리진 서버에서 다운받아 사용자에게 전달하고, 이후 동일한 요청을 받으면 캐싱된 콘텐츠를 사용자에게 제공함.
![[Pasted image 20221124141130.png]]

#### 📌 Amazon CloudFront
- 서비스 설명
![[Pasted image 20221124141748.png]]

#### 📌 CloudFront의 주요기능
1) 정적 콘텐츠에 대한 캐싱 서비스와 비디오 스트리밍 서비스
- 온디맨드 스트리밍 서비스 제공
추가 : [온디맨드(On-Demand) 서비스란?]
![[Pasted image 20221124142542.png]]
![[Pasted image 20221124142640.png]]

2) 동적 콘텐츠에 대한 캐싱 서비스
![[Pasted image 20221124142727.png]]

3) 다양한 보안 서비스
![[Pasted image 20221124142920.png]]

4) 비용최적화
CouldFront비용만 지불하면됌
![[Pasted image 20221124142947.png]]

📌 실습 :Amazon S3 정적 웹 사이트 구성하기 (P.257)
📌 실습 :CloudFront 웹 배포 생성 후 S3와 연결하기(P.239)
📌 실습 : S3 버킷/CloudFront 삭제 (P.270)

# 10장 클라우드 자원과 리소스 관리하기
## IAM (Identiy & Access Management)
#### 📌 IAM이란?
- 일반적인 환경과 웹 서비스 환경에서도 IT관리에 대한 통합 계정 관리 지원

#### 📌 계정 관리 시스템 종류
![[Pasted image 20221124144506.png]]

#### 📌 IAM 서비스
- 리소스에 대한 액세스를 안전하게 관리할 수 있게 해주는 서비스로 그룹을 만들어 관리하며, 권한을 사용해 액세스 허용 및 거부를 할 수 있음
![[Pasted image 20221124144641.png]]
![[Pasted image 20221124144654.png]]

#### 📌 주요특징
1) IAM 사용자와 그룹 정의
- IAM 그룹, LAM 역할과 사용자 계정을 추가하여 권한 및 자격증명을 할당할 수 있음
![[Pasted image 20221124144808.png]]![[Pasted image 20221124144957.png]]

2) 동작방식
![[Pasted image 20221124145101.png]]
![[Pasted image 20221124145027.png]]

3) IAM의 자격증명 관리 기능
- 임시적으로 자격증명 관리가능
![[Pasted image 20221124145219.png]]

📌 실습 : IAM User 및 Group 생성 (P.282)
📌 실습 : IAM Role 생성 및 IAM Role 정책을 통한 EC2 권한 할당 (P.290)

# 나중에 정리할 내용들
서버리스
- [[AWS] 서버리스를 위한 RDS Proxy서비스. RDS Proxy 2019년 12월 3일에 발표된 신규 AWS… | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4%EB%A5%BC-%EC%9C%84%ED%95%9C-rds-proxy%EC%84%9C%EB%B9%84%EC%8A%A4-fb5815b83cce)
- [AWS Serverless를 위한 험난한 여정 — Part 1 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-serverless%EB%A5%BC-%EC%9C%84%ED%95%9C-%ED%97%98%EB%82%9C%ED%95%9C-%EC%97%AC%EC%A0%95-part-1-56ab4be383f3)

람다
- [AWS Lambda를 시작하기 전 알았으면 좋았을것들. 제가 처음 Serverlesss를 접했을때 생각했습니다. 정말 힙하고… | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-lambda%EB%A5%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EC%A0%84-%EC%95%8C%EC%95%98%EC%9C%BC%EB%A9%B4-%EC%A2%8B%EC%95%98%EC%9D%84%EA%B2%83%EB%93%A4-788bd3b3bdd2)
- [비전공자를 위한 AWS Lambda 1편 — 소개. 완전 베이직한 내용과 설명중- 심 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-aws-lambda-1%ED%8E%B8-5697cee473eb)
- [비전공자를 위한 AWS Lambda 2편 — 람다로 문자보내기 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-aws-lambda-2%ED%8E%B8-%EB%9E%8C%EB%8B%A4%EB%A1%9C-%EB%AC%B8-3b73f43d2e20)
- [[번역] AWS Lambda를 VPC를 통해 인터넷 연결하기 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/%EB%B2%88%EC%97%AD-aws-lambda%EB%A5%BC-vpc%EB%A5%BC-%ED%86%B5%ED%95%B4-%EC%9D%B8%ED%84%B0%EB%84%B7-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0-d227e4262238)

기타
- [[AWS] 로드밸런싱 알아보기. 비전공자도 이해할 수 있는 로드밸런싱 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-9fd0955f859e)
- [AWS로 토이프로젝트 운영삽질기. 도커에서 서버리스까지 운영자동화 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws%EB%A1%9C-%ED%86%A0%EC%9D%B4%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%9A%B4%EC%98%81%ED%95%98%EA%B8%B0-5a77f7e13521)
- [클라우드상 오브젝트 스토리지(Object Storage)란? | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C%EC%83%81-%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-object-storage-%EB%9E%80-9d9c2da57649)
- [[AWS] Docker ECS-CLI를 사용하여 도커 띄우기 | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-docker-ecs-cli%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8F%84%EC%BB%A4-%EB%9D%84%EC%9A%B0%EA%B8%B0-54e05178735c)
- [쿠버네티스 CI/DI 를 위한 오픈소스 프로젝트 알아보기. 최근 AWS ECS에서 쿠버네티스로 갈아타며 격동의 연초를 보내고 있는… | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-ci-di-%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-a6657d429c26)