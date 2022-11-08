![](file:///C:/Users/sprae/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)[JVM은 꼭 알아야 합니다..](https://velog.io/@dev_isaac/JVM)     

[JVM, JRE, JDK가 뭔가요? | 얄코 (yalco.kr)](https://www.yalco.kr/42_jvm_jre_jdk/)

**JVM** (Java Virtual Machine) : 자바 가상 머신

WHY JVM 사용해? 한 번 작성해서 어디에서든 실행하게 하는 프로그램(WORA)

코드를 컴파일 언어로 작성하게 되는데, 이를 실행하기 위해서는 컴퓨터 언어인 기계어로 변환한다. 하지만 컴퓨터 환경에 제각각 여서 이를 구애받지 않기 위해 중간단계인 bytecode를 넣은 것이다.

**JRE**(Java Runtime Environment)

클래스 라이브러리를 통해 작성한 자바 코드를 라이브러리와 결합한 후 JVM에 넘겨 실행하는 역할

WHY JRE? 자바의 자료구조 기능들인 List나 Map, Set 같은 걸 사용할 수 있는 이유는 그것들이 바이트코드로 컴파일된 클래스로 제공이 되기 때문이다.

![](file:///C:/Users/sprae/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

**JDK** (Java Development Kit)

자바 개발 도구의 약자임.

개발할 때 필요한 툴 예를 들면,

자바 코드를 컴파일할 때 쓰는 javac

자바를 디버깅할 때 쓰는 jdb        

서로 연관 있는 클래스들을 하나의 jar파일로 묶어주는 jar

![](file:///C:/Users/sprae/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)**Runtime Data Area**

**1)**   **Method Area****(****모든 Thread 공유)**

1. 필드 정보 : 멤버변수 이름, 데이터 타입, 접근 제어자 등의 정보

2. 메서드 정보 : 메서드 이름, 리턴타입, 매개변수, 접근제어자 등의 정보

3. Static 변수

- 모든 객체가 공유 할 수 있고, 객체 생성없이 접근 가능

❓why? 저장하는거야

이 영역에 등록된 class만 heap영역에서 생성 가능

**2)**   **Heap Area****(****모든 Thread 공유)**

1. 객체(인스턴스 - new 연산자로 생성된 객체)가 저장되는 영역

2. 프로그램 실행 중 생성되는 모든 객체는 Heap 영역에 동적으로 할당

- Garbage Collector를 통해 메모리 반환

3. 상수풀 : String literal로 생성한 객체는 상수풀에 들어감

4. 참조형(Reference Type)의 데이터 타입을 갖는 객체(인스턴스), 배열 등은 Heap 영역에 데이터가 저장된다.

**3)**   **Stack Area****(****각 Thread 존재)**

메소드 호출 시 생성되는 스레드 수행정보를 기록하는 Frame을 저장

메소드 정보, 지역변수, 매개변수, 연산 중 발생하는 임시 데이터 저장

기본(원시)타입 변수는 스택 영역에 직접 값을 가진다.

참조타입 변수는 힙 영역이나 메소드 영역의 객체 주소를 가진다.