# 첫 사용자 1명일때
https://youtu.be/8uesJLEXxyk?t=342
## 서버 띄우는 방법
EC2를 이용해서 서버 하나 띄우기
![[Pasted image 20230806140629.png]]

## 서버의 성능 향상 시키는 법은?
- 스케일 업을 이용해서 간단하게 AWS 서버 성능 향상 시키는 방법
![[Pasted image 20230806140917.png]]

# 사용자가 수십명일때
 https://youtu.be/8uesJLEXxyk?t=607

## 기존의 문제점은?
EC2 하나가 죽으면 모두(웹서버, DB, WAS)가 작동이 안됌.
 ![[Pasted image 20230806141120.png]]

## 해결책은?
- 웹서버와 DB서버를 나눈다.
![[Pasted image 20230806141325.png]]
이때, DB인스턴스를 생성해도 되지만, AWS의 DB서비스를 사용하는 방법이 있음.
![[Pasted image 20230806141432.png]]
![[Pasted image 20230806141502.png]]

## RDB와 NOSQL중 어떤걸 선택해야할까?
![[Pasted image 20230806141603.png]]
다음과 같을 때, NoSQL을 사용을 고려해보자.
1) 갑작스럽게 데이터가 증가하는 경우
2) 유저의 write가 많은 서비스
![[Pasted image 20230806141733.png]]

# 사용자가 수백명일때
https://youtu.be/8uesJLEXxyk?t=817

## 아키텍처 구조는?
개발자는 애플리케이션에 집중하고, DB운영은 AWS에게 맡긴다.
![[Pasted image 20230806141922.png]]

 ## 현재 아키텍처 구조의 문제점은?
 웹서버가 하나라서 웹서버가 죽으면 복구 어려움.
 ![[Pasted image 20230806142126.png]]


# 사용자가 수천명일때
https://youtu.be/8uesJLEXxyk?t=858

## 아키텍처 구조는?
- 서버를 두개로 나누고 로드 밸런싱을 통해서 트래픽을 분배함.
![[Pasted image 20230806142239.png]]


# 사용자가 수만명일때
https://youtu.be/8uesJLEXxyk?t=948

## 아키텍처 구조는?
사용자가 늘어남에 따라서 EC2와 RDS를 늘리면 됌.
![[Pasted image 20230806142459.png]]

## ELB를 최적화를 위해 해야하는 것은?
ELB 인스턴스의 성능을 최대한 끌어 올리기 위해 연관 없는 파일은 다른 서비스로 옮겨야함.
![[Pasted image 20230806142730.png]]

1) 정적 콘텐츠를 S3로 이전하기
![[Pasted image 20230806142942.png]]

2) DB의 부하를 줄이기 위해 캐시를 사용함.
![[Pasted image 20230806143156.png]]

3) 동적인 컨텐츠도 CloudFront로 보내자
![[Pasted image 20230806143316.png]]


## 오토 스케일링 적용해보자
사용량에 따라서 자동으로 스케일-인/아웃되고록 설정하지
![[Pasted image 20230806143407.png]]
![[Pasted image 20230806143440.png]]


## 클라우드를 최적화 하는 방법은?
1) 웹서버 경량화
 Apache에서 동시처리에 유리한  NginX로 웹서버 교체
![[Pasted image 20230806151524.png]]

2) 캐시 도입
캐시를 도입하므로써 웹서버와 DB서버에 가해지는 부하를 분산함
![[Pasted image 20230806151654.png]]


3) 인스턴스 타입 변경
부하가 줄어 인스턴스 타입을 내릴 수 있었음. 그로 인해 비용 감소함
![[Pasted image 20230806151934.png]]


# 사용자가 백만명일때
https://youtu.be/8uesJLEXxyk?t=2022
## 아키텍처 구조는?
![[Pasted image 20230806144035.png]]

## 클라우드 네이티브란?
클라우드 네이티브(Cloud-native)는 **애플리케이션 개발 및 배포를 최적화**하기 위해 클라우드의 기능과 환경을 적극 활용하는 방식입니다. 클라우드 네이티브 애플리케이션은 자체 구조와 프로세스에서부터 고가용성, 탄력성, 확장성을 고려하여 설계되며, 지속적인 통합 및 배포가 가능합니다.

1. **마이크로서비스 아키텍처**: 애플리케이션을 작은, 독립적으로 실행되는 서비스로 분할하여 개발하고 관리하는 방식입니다. 이를 통해 개발, 배포 및 유지보수가 용이합니다.

2. **컨테이너화**: 도커(Docker)와 같은 컨테이너 기술을 사용하여 애플리케이션과 그 실행 환경을 패키징하고 격리합니다. 이를 통해 환경간 일관된 배포와 운영이 가능해집니다.

3. **오케스트레이션**: 쿠버네티스(Kubernetes)와 같은 오케스트레이션 플랫폼을 사용하여 컨테이너화된 애플리케이션의 배포, 관리, 스케일링을 자동화합니다.

4. 데브옵스(DevOps)와 지속적인 통합 및 배포(**CI/CD**): 개발 팀과 운영 팀 사이의 협업을 강조하며, 지속적인 통합, 지속적인 배포, 지속적인 모니터링을 통해 개발의 효율성과 안정성을 향상시킵니다.

5. **확장성, 장애 대응, 자동 복구**: 클라우드 인프라를 활용해 자동 스케일링, 데이터 복제 및 분산, 장애에 대한 자동 복구 기능을 구현합니다.


## 클라우드 네이티브 전략은?
![[Pasted image 20230806204850.png]]

- 배포 자동화를 위한 서비스
![[Pasted image 20230806204912.png]]

- 모니터링
![[Pasted image 20230806205024.png]]

- 서비스 재활용으로 개발 비용 낮춤
![[Pasted image 20230806205107.png]]
![[Pasted image 20230806205200.png]]

# 사용자가 오백만일때
https://youtu.be/8uesJLEXxyk?t=2351
## 아키텍처 구조는?
![[Pasted image 20230806205237.png]]
![[Pasted image 20230806205308.png]]


## 개발자가 서버를 관리하지 않는 방법은?
![[Pasted image 20230806210514.png]]

![[Pasted image 20230806210550.png]]


![[Pasted image 20230806210701.png]]


![[Pasted image 20230806210719.png]]