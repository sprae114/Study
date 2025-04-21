#gradle
[[10분 테코톡] 루나의 Gradle - YouTube](https://www.youtube.com/watch?v=ntOH2bWLWQs&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9C%ED%85%8C%ED%81%AC)
[[10분 테코톡] 메리의 Gradle (youtube.com)](https://www.youtube.com/watch?v=V4knLFDG-ZM&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9C%ED%85%8C%ED%81%AC)

-----

# Gradle 개념
## gradle이란?
**빌드 자동화 도구**로서, 소스 코드를 컴파일하고 테스트하고, **필요한 라이브러리를 관리하고, 실행 가능한 애플리케이션으로 만드는 과정**을 자동화해주는 도구입니다.

JVM 상에서 실행되는 스크립트 언어로, Java와 유사한 문법 구조를 가지며 호환성이 좋습니다.


## 빌드란?
개발자가 작성한 소스 코드를 **실행 가능한 소프트웨어로 만드는 과정**을 의미합니다. 
보통 컴파일, 링킹, 패키징 등의 과정을 포함하며, 이를 통해 소스 코드는 사용자가 직접 실행할 수 있는 애플리케이션 형태(`.jar`, `.war`)로 변환됩니다.
![[Pasted image 20240207120304.png]]


## gradle이 나온 이유
1. **라이브러리 관리의 효율성**
기존에는 라이브러리를 일일이 다운로드하고 추가하는 번거로움이 있었습니다. Gradle은 이를 자동화하여 라이브러리를 효율적으로 관리할 수 있게 해줍니다.

2. **버전 관리의 효율성**
프로젝트를 진행하면서 라이브러리의 버전을 동기화하고 관리하는 것은 까다로운 작업입니다. Gradle은 이를 쉽게 해결하고, 팀원들 간의 버전 충돌 문제를 최소화합니다.

3. **빌드 속도 향상**
Gradle은 점진적인 컴파일 기능을 지원하여 변경된 부분만 컴파일함으로써 빌드 시간을 단축시킵니다.


## gradle의 장점
1. **프로젝트 설정의 유연성**
Gradle은 Groovy 기반의 DSL(Domain-Specific Language)을 사용하여 빌드 스크립트를 작성하므로, 프로젝트 설정에 많은 유연성을 제공합니다. 이를 통해 사용자 정의 작업을 쉽게 추가하거나 기존 작업을 수정할 수 있습니다.

2. **멀티 프로젝트 빌드 지원**
Gradle은 하나의 저장소 내에서 여러 하위 프로젝트를 구성할 수 있습니다. 이는 대규모 프로젝트에서 효과적으로 사용되며, 공통되는 부분과 개별적인 부분을 적절히 분리하여 관리할 수 있습니다.
![[Pasted image 20240207121930.png]]

![[Pasted image 20240207121955.png]]


## 기본 gradle 구성
[[Gradle] gradle 설정 방법 (velog.io)](https://velog.io/@iniestar/gradle-tutorial)
[[Gradle] Dependency Configuration이란?](https://ttl-blog.tistory.com/1272)

#### plugins
```gradle
plugins {  
    id 'org.springframework.boot' version '2.6.3'  
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'  
    id 'java'  
}

group = 'com.hello' //프로젝트의 그룹 ID를 설정
version = '0.0.1-SNAPSHOT'  //프로젝트의 버전을 설정
sourceCompatibility = '11' //프로젝트의 Java 소스 코드가 호환되는 Java 버전을 설정
```
**전체적인 어플리케이션 사용 환경을 설정**하는 부분임. 
Spring Boot와 Java를 사용하는 프로젝트를 빌드하고 관리하기 위한 필수적인 플러그인들을 설정하고 있음.


#### 의존성 설정
```gradle
configurations {  
    compileOnly {  
       extendsFrom annotationProcessor  
    }  
}

dependencies {  
    implementation 'org.springframework.boot:spring-boot-starter-web'  
    compileOnly 'org.projectlombok:lombok'  
    developmentOnly 'org.springframework.boot:spring-boot-devtools'  
    annotationProcessor 'org.projectlombok:lombok'  
    testImplementation 'org.springframework.boot:spring-boot-starter-test'  
}
```
1. `configurations` : 이 섹션은 **의존성들이 어떻게 그룹화되고, 어떤 작업에 적용(빌드, 테스트, 런타임 등)되는지를 정의**합니다.

2. `dependencies` : **실제 프로젝트에서 사용될 구체적인 라이브러리나 모듈을 정의**함. 

![[Pasted image 20240207205106.png]]
- `implementation` : A모듈이 변경되었을 경우 재빌드 시 **직접적으로 의존하고 있는 B모듈 까지만 재빌드** 한다. 지정한 모듈까지만 빌드되어 포함되기 때문에 compile보다 빠름.

- `compile` : **의존관계가 있는 모든 모듈을 재빌드**해야 하고 빌드결과물에 포함하기 때문에 A,B,C의 모든 API를 접근할 수 있음(**취약점 때문에 현재 Deprecated**)

- `compileOnly` : 컴파일 시에만 빌드하고 빌드 결과물에는 포함되지 않음(ex. lombok)
why? 컴파일된 바이트코드에 이미 Lombok이 생성한 코드가 포함되어 있기 때문임.

- `runtimeOnly` : 런타임 시에만 필요한 라이브러리인 경우(ex. h2)

- `annotationProcessor` : 어노테이션을 사용하는 라이브러리인경우 명시(ex. lombok)

- `testImplementation` : 테스트 코드를 작성하고 실행할 때 필요한 라이브러리(예: JUnit, Mockito)일 때 사용

#### 저장소
```gradle
repositories {  
    mavenCentral()  
}
```
Gradle 빌드 스크립트에서 사용되며, **의존성을 다운로드 받을 저장소**를 지정하는 부분입니다.

#### 테스트 작업 설정
```gradle
tasks.named('test') {  
    useJUnitPlatform()  
}
```
Gradle 빌드 스크립트에서 **테스트 작업을 설정**하는 부분입니다. 해당 태스크에서 JUnit 플랫폼을 사용하도록 설정합니다.