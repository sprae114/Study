# 01 AWS 이해  
  
## 8 네트워크 기본기 - 패킷의 여행  
#### 📌학습목표는?
=> **컴퓨터 통신원리**를 알아보자

### 8.1 패킷  
#### 📌서킷 스위칭이란?
=> **하나의 회선**을 할당받아 데이터를 주고받는 방식.**직거래** 방식으로 생각하자
![[Pasted image 20230508092709.png]]


#### 📌서킷 스위칭의 문제점은?
=> 사람이 많아지면 물리적 선이 늘어나 비용 증가함.


#### 📌패킷 스위칭이란?
=> 송신 측에서 모든 메시지를 일정한 크기의 패킷으로 **분해해서 전송**하고, 수신측에서 이를 원래의 **메시지로 조립**하는 방식.**택배** 방식으로 생각하자
![[Pasted image 20230508092817.png]]

이를 위해서 다음과 같은 문제들을 해결해야함
1) **어디로 보내야 되는거지?**
=> 최종 목적지에 가장 가까운 **라우터**로 데이터를 보냄(패킷 포워딩)
![[Pasted image 20230508093811.png]]

2) **어떻게 조립할거지?**
=> 어떻게 조립할건지 정보를 담아두는 **헤더** 넣기
![[Pasted image 20230508093824.png]]


#### 📌패킷을 쪼개지 않고 전송하면?
=> A라는 큰 데이터가 있을때, 나머지 B,C 는 **wait이 걸려 동시에 전송 못함**.


#### 📌IP 주소란?
=> 수많은 컴퓨터 구별하기 위해 사용되는 **유일한 값(주소)** 이다.기존에는 IPv4 방식사용했다가 컴퓨터가 많아지면서 IPv6 방식으로 늘리는 중임.


#### 📌포트 번호란?
=> IP 주소로 컴퓨터까지는 잘 도착했는데 **어떤 프로그램에 데이터를 전송**해야하는지 알수 없음.즉 집에 택배가 왔는데 누구꺼인지 알수 없는 상황임.그래서 포트번호를 사용해 어떤 프로그램에 사용하는지 알려줌.(**택배 수령인**)


## 9 EC2 서버 방화벽  

#### 📌EC2의 방화벽이란?
=> 컴퓨터를 임대했으니 로컬 컴퓨터에서 **클라우드 컴퓨터에 원격으로 접속해야함**.하지만 기본 값이 클라우드 컴퓨터는 모든 포트가 차단된 방화벽을 가지고 있음.그래서 22번 포트를 개방해서 SSH를 사용해야함.그래야 원격으로 shell 명령이 가능해짐.
![[Pasted image 20230508100315.png]]

#### 📌방화벽이란?
=> 미리 정의된 보안 규칙으로, **인바운드(들어오는)와 아웃바운드(나가는)** 네트워크 트래픽을 모니터링 제어하는 보안 시스템.
![[Pasted image 20230508100506.png]]

#### 📌Secure가 붙은 이유는?  
=> password같은 민감한 정보를 전송할 수 있음.그래서 security를 적용해 **데이터를 암호화해서 전송**함.


## 10 RSA 인증방식  
#### 📌암호화 방식에 대해서 알아야하는 이유는?
=> SSH에 접속하기 위해 **private key를 등록**해야 했음.이 키를 통한 **인증 방식 원리**에 대해서 알아보자.

#### 📌대칭키 암호화 방식  
=> 암호화와 복호화에 **같은 암호키**를 사용하는 알고리즘.계산속도가 빠르다
![[Pasted image 20230508101846.png]]

하지만, 치명적인 보안 문제가 있음.
1) 열쇠를 보내지 않는다면?
=> 데이터를 K열쇠로 암호화해서 보내면, 열쇠가 없는 B, C 모두 데이터를 볼 수 없음.
![[Pasted image 20230508102120.png]]

2) 열쇠를 보낸다면?
=> 열쇠를 통신으로 보낼수 밖에 없음.그러면 C가 가로챌 위험있음.
![[Pasted image 20230508102120.png]]


#### 📌공개키 암호화 방식(RSA)  
=> 암호화와 복호와에 이용하는 **키가 다른 방식**으로 되어있는 것을 말합니다.예를 들어, A와 B가 데이터를 주고 받기 위해 각자 공개키와 비밀키를 하나씩 만듭니다.

다음과 같은 문제점들을 해결해야 됩니다.
1) 어떤 공개키를 사용해야하는가?
예시) A공개키하면 B와 C 둘다 못품
![[Pasted image 20230508124418.png]]

2) 탈취하면 어떻게 하는가?
예시) B공개키를 C가 탈취해도 데이터 보안 유지 하지만 A는 틀린 이유를 알 수 없음
![[Pasted image 20230508124429.png]]

즉, 다음과 **두번 패킹된 형태**가 되어야함.
![[Pasted image 20230508124438.png]]
![[Pasted image 20230508125157.png]]

#### 📌AWS와 연관짓기
![[Pasted image 20230508125329.png]]
위의 RSA 방식으로 인증을 완료하고 나면, 클라이언트와 EC2 **양쪽에 세션이 생성**되고 이후 자유로운 통신 가능하게 됌.


# 02 리눅스 명령어 학습  
## 리눅스 명령어 step 1
#### 📌clear  
=> 터미널 비울때

#### 📌pwd  
=> 현재 경로 

#### 📌cd  
=> 폴더 이동
- `cd ..`  한칸 상위 폴더로 이동
- `cd (경로)`  해당 폴더로 이동
- `cd /`   최상위 경로로 한 번에 이동

#### 📌ls  
=> 현재 폴더에 있는 모든 파일과 폴더 확인
- `ls -l`  파일 상태 확인 가능
- `ls --all`, `ls -a` 숨긴 파일을 포함한 모든 파일 보여줌

#### 📌절대 경로와 상대 경로  
현재 `/etc`에 있는데, `/home/ubuntu`로 가는 방법은?

1) 절대경로 `cd /home/ubuntu`

2) 상대경로 `cd /` -> `cd home/ubuntu`
  
## 리눅스 명령어 step 2  
#### 📌--help  
=> 명령어 사용법 알려줌

#### 📌mkdir  
=> 폴더 생성 `mkdir (폴더명)` 

#### 📌touch  
=> 파일 생성 `touch (파일명.txt)` 

#### 📌rm  
=> 폴더나 파일 삭제
`rm (이름)` 삭제
`rm -f (이름)` 강제 삭제 옵션 
`rm -r (이름)` 폴더 삭제할때, 내부에 파일이 있다면 함께 삭제하는 옵션

#### 📌Ctrl + C  
=> 명령취소

## 리눅스 명령어 step 3  
#### 📌cp(copy)  
=>파일 복사 `cp (복사할 파일명) (생성할 파일명)`

#### 📌mv
 `mv (이동전 파일명) (이동후 위치)`  파일 이동
 `mv (변경전 파일명) (변경후 파일명)`  파일명 변경

  
## 리눅스 명령어 step 4  
#### 📌Windows에서 프로그램을 설치한다는 의미는?

프로그램을 하드디스크에 복사, 복사된 위치에 찾아가서 실행해야 하면 불편하니까 운영체제(윈도우, 리눅스,맥)에 환경변수 등록, 방화벽 포트 개방 (통신을 위해서), 부팅 후 운영체제가 시작될 때 자동으로 실행될 수 있도록 설정 등등 많은 과정이 사실 필요함.(**요리 안에 식품들을 하나씩 설명**하는 것으로 생각하자)
![[Pasted image 20230508132909.png]]

이러한 과정을 편리하게 설치하기 위해 다음과 같은 저장소 사용함.
1) ubuntu repository  
=> 저장소를 이용하면 더블 클릭하듯 설치 가능함.**메뉴판**을 이용한다고 생각하자
![[Pasted image 20230508132924.png]]

2) PPA 저장소  
=> ubuntu repository 없을 경우 **개인 저장소** 사용함.

## 리눅스 명령어 step 5  
#### 📌whoami
=> **현재 사용중인 사용자** 확인

#### 📌sudo apt update  
=> 처음 apt목록에는 아무것도 없기때문에 **메뉴판 갱신**해줘야함.

#### 📌apt  
=> **메뉴판 목록조회**
`apt-cache`  apt 메뉴판의 목록조회
`apt-cache search (이름명)` 이름명 이라는 프로그램이 메뉴판에 있는지 검색함 
`apt-cache search (이름명) | (이름명2)` 이름명이라는 검색 결과에 이름명2 조건 추가
`apt list | grep tomcat`  메뉴판 전체보기 + tomcat으로 조건 추가
`sudo apt install (이름명)`  이름명 프로그램 설치하기

#### 📌톰캣 설치
`sudo apt install -y tomcat9`  톰캣9설치
`netstat -nlpt`  8080포트로 잘 돌아가는 것을 확인할 수 있음.
![[Pasted image 20230510185541.png]]


#### 📌포트 확인  
=> 프로그램이 설치되어 있다는 것은 해당 프로그램의 포트가 활성화 되었다는 것을 의미함.
`sudo apt install net-tools` netstat 명령어 설치
`netstat -nlpt`  포트 조회하기 (EC2의 IP주소:포트번호) 

인바운드 규칙 설정  
=> **포트가 할당되어 있지만, 포트를 열지 않아서 접속 불가**함.그래서 AWS에서 인바운드 규칙 추가해야함.
![[Pasted image 20230508224844.png]]
  

## 리눅스 명령어 step 6  
#### 📌 프로그램 삭제  
`sudo apt remove tomcat9`  톰캣9(프로그램명)을 삭제
`sudo apt --purge remove tomcat9`  톰캣9(프로그램명)에 설정파일까지 삭제


#### 📌 PPA란?
=> 기존의 ubuntu repository 저장소가 특정 프로그램(ex. tomcat8)을 가지고 있지 않기 때문에 저장소를 하나 추가해주는 것을 말함. (PPA에서 찾는 사이트  [Ubuntu in Launchpad](https://launchpad.net/ubuntu))


#### 📌 PPA 명령어
참고 : [Crocus](https://www.crocus.co.kr/1592)
```
sudo add-apt-repository 저장소 이름
sudo apt-get update
sudo apt-get install 프로그램명
 
# 삭제를 원할 시 
sudo add-apt-repository --remove 저장소 이름
```
저장소 추가한 후, 기존 apt이용한 것처럼 설치하자


## 리눅스 명령어 step 7  
#### 📌프로세스(Process)  
![[Pasted image 20230508225033.png]]
1. CPU (Central Processing Unit): CPU는 컴퓨터의 두뇌로 불립니다.모든 프로그램과 **애플리케이션의 명령을 해석하고 실행하는 역할**을 합니다.CPU는 연산 속도와 명령 처리 능력에 따라 컴퓨터의 전체 성능에 큰 영향을 미칩니다.

2. RAM (Random Access Memory): RAM은 컴퓨터의 주 기억장치로, **CPU에 의해 처리되는 프로세스와 데이터를 일시적으로 저장하는 역할**을 합니다.RAM은 CPU와 빠른 속도로 데이터를 주고받을 수 있으며, 프로세스 처리 속도와 컴퓨터의 다중 작업 능력에 큰 영향을 미칩니다.

3. HDD (Hard Disk Drive): HDD는 컴퓨터의 보조 기억장치로, **데이터와 프로그램을 영구적으로 저장**합니다.HDD의 저장 용량과 속도는 컴퓨터의 전체 성능과 응용 프로그램의 로딩 및 저장 시간에 영향을 미칩니다.

#### 📌스레드(Thread)  
=>**CPU는 일하는 노동자**가 한명이라는 것임. 일꾼이 한개면 한개의 프로세스밖에 구동 못함.그래서 두개의 프로그램을 실행하기 위해서는 다음 그림과 같이 왔다갔다 하면서 일해야함.
![[Pasted image 20230508225054.png]]
=> 이렇게 **왔다갔다 하는 것을 컨텍스트 스위칭**이라함.컨텍스트 스위칭을 하기 위해서는 다음과 같은 사항을 알고 있어야함.

1) **어디서부터 다시 시작**할까?(흐름)
![[Pasted image 20230508231854.png]] 

2) **어떻게 전환**할 수 있을까?(sleep)
=>왔다갔다 하기 위해서는 자원을 100%로 활용하면 안되고, 잠깐의 sleep이 필요함.(준비시간) 
![[Pasted image 20230508232012.png]]


## 리눅스 명령어 step 8  
#### 📌service 
=> **서비스를 관리**하기 위한 명령어
```
정리 서비스 목록 보기
service --status-all

서비스 종료
sudo service tomcat8 stop

서비스 시작
sudo service tomcat8 start
```

#### 📌systemctl
=> **서비스를 관리**하기 위한 명령어
```
실행 중인 시스템 목록
sudo systemctl list-unit-files

실행 중인 시스템 목록중 tomcat8 검색
sudo systemctl list-unit-files | grep tomcat8

tomcat8 시스템 상태 확인
sudo systemctl status tomcat8

tomcat8 프로세스 종료
sudo systemctl stop tomcat8

tomcat8 프로세스 실행
sudo systemctl start tomcat8
```


#### 📌service대신에 systemctl를 사용하는 이유는?
1.  더 효율적인 동시성: Systemd는 서비스를 병렬로 시작하여 부팅 시간을 줄입니다. 이는 서비스 간의 의존성을 명확하게 처리할 수 있게 해줍니다.
2.  쉬운 관리: systemctl 명령어를 사용하여 서비스를 시작, 중지, 재시작 및 상태 확인과 같은 작업을 간단하게 수행할 수 있습니다.
3.  자동 재시작: 서비스가 실패할 경우, Systemd는 자동으로 재시작하여 시스템 안정성을 향상시킵니다.
4.  빠른 로깅 및 진단: Systemd는 서비스 로깅을 처리하기 위해 journal이라는 중앙 집중식 로깅 시스템을 사용합니다. 이를 통해 로그 관리 및 진단이 간편해집니다.


#### 📌ps
=> 지금 **실행중인 프로세스의 목록** 볼 수 있음
```
실행 중인 프로세스 목록
ps -ef

tomcat PID 검색
ps -ef | grep tomcat8

grep 프로세스 제외하고 검색
ps -ef | grep tomcat8 | grep -v
```


#### 📌kill
```
종료하는 방법에 대한 옵션 목록
kill -1

PID 강제 종료
sudo kill -9 pid값

PID 안전한 종료
sudo kill -15 pid값
```


#### 📌restart
![[Pasted image 20230511003210.png]]

```
tomcat8 프로세스 재시작
sudo systemctl restart
```


#### 📌pid 찾는 script 만들기
```
awk와 백틱으로 tomcat8 PID 값 찾아서 종료
sudo kill `ps -ef grep tomcat8 | grep -v grep awk '{print $2}'`
```


## 리눅스 명령어 step 9  
#### 📌vi
```
vi a.txt    파일 생성 및 vim 시작
cat a.txt    파일 내용 출력
```

▶일반 모드  
```
ESC    일반 모드 진입방법
마우스 우클릭    외부 코드 붙여넣기  
shift + v    블록 지정  
x    현재 커서의 문자 삭제  
dd    현재 행 삭제  
yy    현재 행 복사  
p    붙여넣기  
home    행의 맨 처음으로 이동 
$    행의 맨 끝으로 이동  
ctrl + b    위로 한 화면 스크롤  
ctrl + f    아래로 한 화면 스크롤
```

▶입력모드
```
i/a    입력모드 진입 방법
```

▶명령행 모드  
```
esc 입력후 :    명령행모드 진입 방법
:w    저장
:q    에디터 종료
:q!   강제 에디터 종료
:wq   저장 후 종료
```

## 리눅스 명령어 step 10  
#### 📌root 접속  
```
su root    root로 접속
sudo passwd root    root비밀번호 변경
```


#### 📌권한이란?
![[Pasted image 20230512043946.png]]
=> 파일을 생성하면 다음과 같이 권한과 생성된다. 생성되는 권한에 대해서 알아보자

![[Pasted image 20230512044040.png]]

▶ 권한의 3가지 종류
- 파일 읽기(r)
- 파일 쓰기(w)
- 파일 실행하기(x)

▶숫자로 권한 표기하기
=> 위에 예시는 644권한을 가진다고 말할 수 있다.
![[Pasted image 20230512051417.png]]

if 파일을 수정하고 싶다면? 646으로 변경해야함.
![[Pasted image 20230512051523.png]]

#### 📌그룹
![[Pasted image 20230512051801.png]]
=> B,C가 권한이 없기 때문에 A의 모든 권한을 주기 위해서 그룹 만드는 방법 설명

```
sudo groupadd [그룹명]    새로운 그룹을 생성
sudo usermod -a -G [그룹명] [사용자명]    해당 그룹에 사용자를 추가
groups [사용자명]    그룹 확인
```


#### 📌chmod
=> **권한 부여**하는 명령어 ( u:소유자, g:그룹, o:누구나 )
```
sudo chmod 646 test1.txt    수정 권한을 부여하는 명령어
sudo chmod 777 test1.txt    모두에게 권한을 부여하는 명령어(파일명이 초록색 됌)
sudo chmod u+x,g+wx,o+x index.html    숫자를 사용하지 않고 권한 추가(775) u:소유자, g:그룹, o:누구나
sudo chmod o=rw index.html    권한을 덮어 씌울때 사용
```

## 리눅스 명령어 step 11  
#### 📌chown 소유자: 그룹 변경  
```
sudo chown [파일소유자]:[그룹명] [파일명]   소유자: 그룹을 변경
```  


## 리눅스 명령어 step 12  
#### 📌파일 찾기  
```
sudo find / -name [서비스명]    /최상위부터 찾으며, [서비스명] 으로 찾겠다는 옵션
```


#### 📌포트 변경  
```
cd conf    톰캣 내부 폴더로 진입
sudo vi server.xml    파일 진입후, port:8080 -> 8000로 변경하기
```

하지만 접속이 안된다. 그 이유는
1) 재시작해야함
```
sudo service tomcat8 restart    톰캣 재시작
```

2) 인바운드 규칙 추가
=> 인스턴스에 가서 인바운드 규칙 8000 추가해주기
  

## 리눅스 명령어 step 13  
#### 📌tail  
=> 파일의 **가장 끝 부분의 10줄을 출력**해 주는 명령
```
sudo find / -name catalina.out    로그 파일 위치 찾기
cd /var/log/tomcat8     방금찾은 위치 들어가기
sudo tail -f catalina.out    실시간으로 로그 보여주기
이후 마우스 우클릭 후 Duplicate tab을 실행해, 톰캣을 재시작하면(sudo service tomcat8 restart) 실시간로그 확인가능
```


#### 📌표준 입출력 변경 
=>표준 출력을 catalina에서 **다른 파일로 출력** 바꾸기
```
sudo touch mylog.out    파일 생성
sudo chmod 777 mylog.out     권한 부여
sudo tail -f catalina.out > mylog.out     파일 입출력 변경
이후, mylog파일을 확인해보면 로그가 쌓여있음
```

# 03 AWS EC2 기본 배포하기  
  
## 배포 V1 흐름 이해하기  
#### 📌기본이 되는 배포 V1  
1) github에 업로드 파일을 EC2에 다운로드
2) 서버 실행을 위한 설정(tomcat, JDK ...)
![[Pasted image 20230508232645.png]]
  
## 3 EC2 서버 생성 및 고정 IP 설정
#### 📌EC2 서버 생성 설정
- OS : Ubuntu
- 인스턴스 : t2.micro
- keypair 생성하기
- 인바운드 규칙 8080추가
- 스토리지 30GB

#### 📌고정 IP와 탄력적 IP이란?
▶고정 IP
=> 서비스 하기 위해 사용하는 고정IP. why? 클라이언트는 상관없는데 서버는 IP주소가 바뀌면 찾을 수가 없음.

▶탄력적 IP
=> 모든 집에서 지속적으로/동시에 인터넷을 사용하지 않기때문에 **공유**해서 사용함.
![[Pasted image 20230508233333.png]]

![[Pasted image 20230508233248.png]]

#### 📌탄력적 IP 생성  및 연결

![[Pasted image 20230508235838.png]]
![[Pasted image 20230508235911.png]]
  
## 4 프로젝트 배포하기 V1  
#### 📌JDK 설치  
`sudo apt update` 메뉴판의 목록을 확인
`sudo apt-cache search jdk | grep openjdk-11`  JDK 11버전 찾기
`sudo apt install openjdk-11-jdk`  JDK 11버전 설치
`java --version`  설치된 자바버전 확인

#### 📌git
`git --version`  깃 버전 및 설치 확인
`git clone (프로젝트 주소)`  프로젝트 내려받기


#### 📌gradlew 실행 권한 부여  
`sudo chmod u+x gradlew`  실행권한 부여함. `ls -l`로 확인해보면 파일이름이 초록색으로 변경됌.
`./gradlew bulid` 실행파일 빌드함. 이러면 build내부에 `.jar` 실행파일  만들어져 있음.


#### 📌Gradle과 Gradlew의 차이는?
-   Gradle은 개발자가 시스템에 직접 설치해야 하는 반면, Gradlew는 프로젝트와 함께 제공되는 실행 가능한 스크립트입니다.
-   Gradle을 사용하려면 개발자가 시스템에 적절한 버전의 Gradle을 설치하고 관리해야 하지만, Gradlew는 프로젝트에 포함된 설정 파일을 통해 자동으로 올바른 버전의 Gradle을 다운로드하고 사용합니다. 이로 인해 프로젝트 빌드의 일관성이 향상됩니다.
-   Gradlew를 사용하면 개발자들이 동일한 Gradle 버전을 사용하여 프로젝트를 빌드할 수 있어, 빌드 환경 간의 차이로 인한 문제를 최소화할 수 있습니다.

**Gradlew는 프로젝트의 빌드 환경을 표준화하고 일관성을 유지**하는데 도움이 되며, 개발자들이 더 쉽게 프로젝트를 빌드하고 테스트할 수 있게 해줍니다. 따라서 **대부분의 경우 Gradlew를 사용**하는 것이 좋습니다.


#### 📌jar 파일 실행
`java -jar (jar파일 이름.jar)`  jar 실행하면 성공적으로 **서버에 띄운것**임. 탄력적 IP주소로 접속해보자. 하지만 **SSH를 종료했을 때, 서버가 같이 종료되는 문제점**이 있음.

#### 📌Gradle 오류
- 내용 : `FAILURE: Build failed with an exception.`
- 이유 : Gradle 빌드 오류는 권한 문제, Gradle 버전과 관련된 문제
1) 권한 문제 해결책
`sudo chown -R ubuntu:ubuntu /home/ubuntu/Blog`  권한 수정하기
2) Gradle 버전 해결책
```
gradle -v  //버전 확인

//최신 버전으로 업데이터 명령어
sudo add-apt-repository ppa:cwchien/gradle
sudo apt-get update
sudo apt-get install gradle
```


## 5 프로젝트 백그라운드로 실행
#### 📌plain.jar 파일 생성되지 않는 코드 추가  
=>**한개의 jar파일**을 생성하기 위함
build.gradle
```
jar{
	enabled=false
}
```

#### 📌nohup  
=>**터미널 창을 종료하더라도 서버에 접속**하기 위함.
```
nohup java -jar *.jar &    백그라운드로 실행
nohup java -jar v1-0.0.1-SNAPSHOT.jar 1>log.out 2>err.out &    서버 재시작
```


#### 📌로그 확인하는 방법은?
1) 로그파일
=> build/libs 위치에 nohup.out파일이 생긴것을 확인할 수 있음.
`cat nohup.out` 로그 확인가능

2) 실시간 로그 모니터링
`tail -f nohup.out` 로그 실시간 확인 가능


## 6 로그 파일 위치 변경  
#### 📌프로세스 종료  
```
ps -ef    PID 찾기
kill -9 [PID]    찾은 PID를 종료시키기
netstat -nlpt    실행중인 프로세스 포트 목록 확인
```

#### 📌로그 파일 변경 후 실행  
```
nohup java -jar *.jar > mylog.out &    로그 출력 파일 변경
tail -f mylog.out    실시간 모니터링으로 로그 확인
```


## 7 표준 출력, 표준 에러  
#### 📌1 표준 출력(1)과 에러 출력(2)  
`nohup java -jar v1-0.0.1-SNAPSHOT.jar 1>log.out 2>err.out &`  표준 출력 및 에러 출력하기.  `cd build/libs` 이동후 해야함.

#### 📌정상 로그와 에러 로그를 분리하는 이유는?
=> 실행이 잘 안된다면, 에러 로그만 확인하고, 잘되면 정상 로그만 확인하면 되기 때문임.
  
## 8 타임존 변경 및 종료 스크립트 작성  
#### 📌timezone 세팅  
`timedatectl` 시간 확인해보기
`timedatectl list-timezones | grep Seoul` 서울 시간 찾아보기
`sudo timedatectl set-timezone Asia/Seoul` 서울 시간으로 세팅

#### 📌pgrep  
=> **PID 찾기** 위한 명령어
```
ps -ef grep *.jar | grep -v grep awk '{print $2}'    기존의 PID를 찾기 위한 명령어
pgrep -f *.jar    PID 찾는 간단한 방법
```

#### 📌echo
=> 표준 출력 모니터로 **글자를 출력**하라는 명령어
`echo "Springboot Stop....." `

#### 📌종료 스크립트 작성  
```
vi spring-stop.sh    종료 스크립트 만들기

스크립트 작성하기
echo "Springboot Stop....." 
Spring_PID=$(pgrep -f .jar) 
echo $Spring_PID
kill -9 $Spring_PID

chmod u+x spring-stop.sh    실행 권한 부여
```

#### 📌서버 재시작  
`nohup java -jar v1-0.0.1-SNAPSHOT.jar 1>log.out 2>err.out & `


## 9 cron 주기적 실행  
#### 📌cron을 배우는 이유는?
=> 배포해보면 부하가 심하거나, 에러가 발생해서 서버를 종료하게 되고 수동 재시작 했어야했다. 이제 자동 재시작하는 방법을 작성해보자


#### 📌cron이란?
=> 시간 기반 잡 스케줄러로, 시간/날짜/간격에 주기적으로 실행할 수 있도로 스케줄링하기 위해 사용함.

```
crontab -e    crontab 파일을 편집
crontab -l    crontab 내용을 확인
```

crontab 파일에는 실행할 명령과 실행 주기를 지정할 수 있는 엔트리들이 포함됩니다.
```
* * * * * command
```
위의 형식에서 각 `*`는 해당 필드의 모든 값을 나타냅니다. 각 필드는 다음과 같은 의미를 가집니다:

1. 분 (0-59)
2. 시간 (0-23)
3. 일 (1-31)
4. 월 (1-12)
5. 요일 (0-7, 0과 7은 일요일)

위의 필드에 숫자 또는 구간을 지정하여 실행 주기를 세밀하게 제어할 수 있습니다. 예를 들어, `*/5`는 5분 간격을 의미하고, `1-5`는 1부터 5까지의 값 범위를 의미합니다.

- 매 분 실행: `* * * * * command`
- 매 시간의 30분에 실행: `30 * * * * command`
- 매일 오전 4시에 실행: `0 4 * * * command`
- 매주 월요일 오전 9시 30분에 실행: `30 9 * * 1 command`
- 매월 1일 오전 6시에 실행: `0 6 1 * * command`

----

9.2 cron 자동화  
  
10 스크립트로 cron 등록  
  
11 cron으로 프로젝트 재시작  
11.1 spring-stop.sh  
11.2 spring-restart.sh  
11.3 deploy  
  
12 재배포 프로세스 이해  
12.1 재배포 프로세스 이해  
  
13 재배포하기  
13.1 기존 서버 중지  
13.2 aws-v1 폴더 삭제  
13.3 프로젝트 다운로드  
13.4 gradlew 실행 권한 부여하기  
13.5 빌드  
13.6 jar 실행시키기  
13.7 cron으로 자동 재시작                 

# AWS RDS 만들기
## 데이터베이스 만들기
참고 : [(4) 아마존 웹 서비스 (AWS) RDS로 MySQL DB 실행하고 접속하기 - YouTube](https://www.youtube.com/watch?v=wdaMD6yQVh4)
#### 1) RDS에 들어가서 데이터베이스 생성 클릭

#### 2) 데이터 베이스 설정 옵션
▶ 데이터베이스 생성 방식 선택 
- 표준생성

▶ 엔진 옵션
- MariaDB, 엔진버전 기억해두기

▶ 템플릿
- 프리 티어

▶설정 
- 인스턴스 식별자 : RDS를 구별하는 용도로 이름
- 마스터 사용자 이름/비밀번호 : DB에 접근할때 필요한 내용임으로 입력후 기억해두기

▶연결
- 컴퓨팅 리소스 : EC2 컴퓨팅 리소스에 연결 안 함, IPv4
- 퍼블릭 액세스 : 예
- 기존 VPC 보안 그룹 : 나중에 컴퓨터나 EC2로 접근성 판단, 일단 default 추후에 보안그룹 추가하기

#### 3) 파라미터 설정하기
▶파라미터 그룹이란?
 Amazon RDS에서 데이터베이스 **엔진의 구성을 관리**하는 데 사용되는 컨테이너입니다. 파라미터 그룹에는 데이터베이스 엔진과 관련된 설정 값들이 포함되어 있으며, 이러한 설정들은 데이터베이스 인스턴스의 성능, 연결, 보안 등과 같은 작업에 영향을 줄 수 있습니다.

▶ 위치
- RDS에서 파라미터그룹 클릭, 파라미터 그룹 생성하기
- 생성한 RDS 인스턴스와 같은 버전 맞추기, 이름은 자유롭게

▶ 타임존
- time_zone을 Asia/seuol 설정

▶ Character Set
- 이모지 저장 가능 여부 설정임
- character 6개 항목을 uf8mb4 설정
- collation 2개 항목을 utf8mb4_general_ci 설정

▶ Max Connection
- 인스턴스 사양에 따라 자동으로 정해지지만, 프리티어는 150하자

#### 4) 파라미터 그룹 수정 적용하기
- 해당 RDS 수정
![[Pasted image 20230509222041.png]]

- 수정한 파라미터 그룹 설정
![[Pasted image 20230509222107.png]]

#### 5) 보안그룹 설정하기
https://youtu.be/wdaMD6yQVh4?t=454 해당 링크보며 따라하기


## 로컬 PC에 DB 접근하기
#### 1) database 선택하기
- RDS를 만들때 선택했던 DB를 선택하자
![[Pasted image 20230509233644.png]]

#### 2) 설정
- host : RDS의 엔드포인트 복사
- 아이디/비번 : 적어둔 아이디/비번 입력
![[Pasted image 20230509233749.png]]

#### 3) 테스트해보기
```sql
show databases;  

use 데이터베이스이름; //사용할 데이터베이스 찾은후 사용

//데이터 베이스 설정 추가
ALTER DATABASE 데이터베이스명
CHARACTER SET = 'utf8mb4'
COLLATE='utf8mb4_general_ci';

//데이터 넣어보기 예시
CREATE TABLE test (  
id bigint(20) NOT NULL AUTO_INCREMENT,  
content varchar(255) DEFAULT NULL,  
primary key (id)  
) ENGINE = InnoDB;  
  
insert into test(content) values ('테스트3');  
  
select * from test;
```

## EC2에서 RDS 접근하기
참고 : [[Ubuntu] 우분투에 MySQL 설치하기 (velog.io)](https://velog.io/@seungsang00/Ubuntu-%EC%9A%B0%EB%B6%84%ED%88%AC%EC%97%90-MySQL-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

#### 1) 우분투 서버 업데이트
`sudo apt-get update`

#### 2) mysql-server 설치
`sudo apt-get install mysql-server`


#### 3) MySQL 기본 설정

```
sudo ufw allow mysql    //외부 접속 기능 설정 (포트 3306 오픈)
sudo systemctl start mysql    //MySQL 실행
sudo systemctl enable mysql    //Ubuntu 서버 재시작시 MySQL 자동 재시작
```

#### 4) MySQL 접속
```
sudo /usr/bin/mysql -u (아이디) -p -h (엔드포인트)
```