-   [자바 컴파일과정](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.md)  
    -   실행과정  
        
        -   그림![](https://api.transno.com/v3/document_image/a5b5345f-cc12-4ab5-b6fe-c000f87fb0c3-10826299.jpg)  
            
        
        -   1) 작성한 자바소스(JAVA Source), 즉 확장자가 .java인 파일을 자바 컴파일러(JAVA Compiler)를 통해 자바 바이트코드(JAVA Byte Code)로 컴파일한다.  
            
        
        -   2) 컴파일된 바이트코드를 JVM의 클래스로더(Class Loader)에게 전달한다.  
            
        
        -   3) 클래스로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올린다.  
            
        
        -   4) 실행엔진(Execution Engine)은 JVM메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.  
            
-   [자바 동기와 비동기 방식](https://webheck.tistory.com/entry/Java%EB%8F%99%EA%B8%B0%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B0%A9%EC%8B%9DAsynchronous-processing-model)  
    
    -   정의  
        
        -   동기식 처리  
            -   직렬적으로 태스크(task)를 수행한다. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다.  
                
        
        -   비동기식 처리  
            -   병렬적으로 태스크를 수행한다. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행한다.  
                
    
    -   장단점  
        
        -   동기식 처리  
            
            -   장점 : 설계가 매우 간단합니다.  
                
            
            -   단점 : 요청에 따른 결과가 반환되기 전까지 아무것도 못하고 대기해야합니다. (블록상태)  
                
        
        -   비동기식 처리  
            
            -   장점 : 요청에 따른 결과가 반환되는 시간 동안 다른 작업을 수행할 수 있습니다. (논블록상태)  
                
            
            -   단점 : 동기식보다 설계가 복잡함.  
                
    
    -   [서버에 적용해보기](https://kimdeveloper.tistory.com/2)  
        
    
    -   구현해보기  
        
        -   동기식 처리  
            -   ![](https://api.transno.com/v3/document_image/677f7a1f-2575-4cd5-afa2-4c39448a3999-10826299.jpg)  
                
        
        -   비동기식 처리  
            -   ![](https://api.transno.com/v3/document_image/a26c9279-984b-4468-a2f1-eb3205d8d2ab-10826299.jpg)  
                
-   OOP  
    -   시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법을 의미하며, 여기서 자율적인 객체란 상태와 행위를 함께 지니며 스스로 자기 자신을 책임지는 객체를 의미합니다.  
        
-   [좋은 객체 지향 설계의 5가지 원칙(SOLID)](https://brownbears.tistory.com/search/solid)  
    
    -   SRP 단일 책임 원칙(single responsibility principle)  
        -   한가지 클래스에는 한가지 책임만 가져야한다  
            -   why? 유지보수를 힘들게 만드는 부분이며 다른 개발자가 핵심 로직을 파악하는데 시간을 많이 소요할 수 있습니다.  
                
    
    -   OCP 개방 - 폐쇄 원칙(open/closed principle)  
        -   확장에는 열려 있으나, 변경에는 닫혀있어야 한다  
            -   why?기능을 추가한다고 할 때, 기존의 코드를 변경하지 않고 변경할 수 있어야함.  
                
    
    -   LSP 리스코프 치환 원칙(Liskov substitution principle)  
        -   상위 타입의 객체를 하위 타입의 객체로 치환해도 동작에 문제가 없어야함.  
            
    
    -   ISP 인터페이스 분리 원칙(interface segregation principle)  
        -   인터페이스를 사용하는 클라이언트 기준으로 분리해야 되는 원칙  
            -   하나의 일반적인 인터페이스 보다는 두개의 구체적인 인터페이스를 사용하자.  
                
    
    -   DIP 의존관계 역전 원칙(dependency inversion principle)  
        -   구현 클래스에 의존하지 말고, 인터페이스에 의존해야함.  
            
-   포인터란?  
    -   어떤 변수의 주소값을 저장하는 변수입니다.  
        
-   Java의 장점과 단점은?  
    
    -   장점  
        
        -   JVM 위에서 동작하기 때문에 운영체제에 독립적이다.  
            
        
        -   가비지컬렉터가 메모리를 관리해주기 때문에 편리하다.  
            
    
    -   단점  
        
        -   JVM 위에서 동작하기 때문에 실행 속도가 상대적으로 느리다.  
            
        
        -   다중 상속이나 타입에 업격하는 등 제약이 있는 것이 많다.  
            
-   Java가 다중 상속을 지원하지 않는 이유는?  
    -   다중 상속을 지원하면 다이아몬드 문제가 발생할 수 있기 때문입니다. 예를 들어 Human 클래스에 있는 walk() 메소드를 Female 클래스와 Male 클래스가 모두 구현하였다고 할 때, Female과 Male 클래스를 다중 상속 받은 Person 클래스의 입장에서는 코드의 충돌![](https://api.transno.com/v3/document_image/b5cb4f4d-882f-4cb3-ae70-310b66b396c6-10826299.jpg)  
        
-   오버라이딩(Overriding)과 오버로딩(Overloading)차이는?  
    
    -   오버라이딩(Overriding): 상위 클래스가 가지고 있는 메소드를 하위 클래스에서 재정의하여 사용하는 기술  
        
    
    -   오버로딩(Overloading): 매개변수의 타입과 개수를 변경하면서 같은 이름의 메소드를 여러 개 사용하는 기술  
        
-   클래스(Class), 객체(Object), 인스턴스(Instance)의 개념은?  
    
    -   클래스(Class): 객체를 만들어내기 위한 설계도 혹은 틀  
        
    
    -   객체(Object): 설계도(클래스)를 기반으로 선언된 대상, 클래스의 인스턴스라고도 부름  
        
    
    -   인스턴스(Instance): 객체에 메모리가 할당되어 실제로 활용되는 실체  
        
-   [추상클래스와 인터페이스의 차이는?](https://brunch.co.kr/@kd4/6)  
    
    -   추상클래스  
        
        -   단일 상속만이 가능하다.  
            
        
        -   모든 접근 제어자를 사용할 수 있다.  
            
        
        -   변수와 상수를 선언할 수 있다.  
            
        
        -   추상 메소드와 일반 메소드를 선언할 수 있다.  
            
        
        -   부분 구현된 메서드를 상속받아 확장시키기 위함  
            
    
    -   인터페이스  
        
        -   다중 구현이 가능하다.  
            
        
        -   public 접근 제어자만 사용할 수 있다.  
            
        
        -   상수만 선언할 수 있다.  
            
        
        -   추상메소드만 선언할 수 있다.  
            
        
        -   구현 객체의 동일성 보장  
            
-   Java의 List, Set, Map 차이  
    
    -   List  
        
        -   데이터를 순차적으로 저장한다.  
            
        
        -   데이터의 중복을 허용한다.  
            
        
        -   데이터로 null을 허용한다.  
            
    
    -   Set  
        
        -   순서없이 Key로만 데이터를 저장한다.  
            
        
        -   Key의 중복을 허용하지 않는다.  
            
        
        -   Key로 null을 허용하지 않는다.  
            
    
    -   Map  
        
        -   순서없이 Key, Value로 데이터를 저장한다.  
            
        
        -   Value는 중복을 허용하지만 Key의 중복을 허용하지 않는다.  
            
        
        -   Key로 null을 허용하지 않는다.  
            
-   Java의 Vector와 ArrayList 차이  
    
    -   Vector  
        
        -   동기화를 지원한다.  
            
        
        -   속도가 느리지만 병렬 상황에서 안전하다.  
            
        
        -   크기가 증가하는 경우, 2배 증가함(10 -> 20)  
            
    
    -   ArrayList  
        
        -   동기화를 지원하지 않는다.  
            
        
        -   속도는 빠르지만 병렬 상황에서 안전하지 않다.  
            
        
        -   크기가 증가하는 경우, 1.5배 증가함(10 -> 15)  
            
-   Java의 StringBuffer와 StringBuilder 차이  
    
    -   StringBuffer  
        
        -   동기화를 지원한다.  
            
        
        -   속도가 느리지만 병렬 상황에서 안전하다.  
            
    
    -   StringBuilder  
        
        -   동기화를 지원하지 않는다.  
            
        
        -   속도는 빠르지만 병렬 상황에서 안전하지 않다.  
            
-   Stream API의 장점과 단점  
    
    -   장점  
        
        -   코드를 간결하게 작성하여 가독성을 높일 수 있다.  
            
        
        -   병렬스트림과 같은 기술을 이용하면 처리 속도를 많이 높일 수 있다.  
            
    
    -   단점  
        -   코드들이 추상화되어 있어 실수가 발생할 수 있다.  
            
-   HashMap vs HashTable vs ConcurrentHashMap  
    
    -   HashMap  
        
        -   동기화 X  
            
        
        -   key, value에 null을 허용O  
            
    
    -   HashTable  
        
        -   동기화 O  
            
        
        -   key, value에 null을 허용X  
            
    
    -   ConcurrentHashMap  
        
        -   HashMap을 thread-safe 하도록 만든 클래스  
            
        
        -   key, value에 null을 허용X  
            
-   JVM 구조  
    
    -   그림![](https://api.transno.com/v3/document_image/88fd4ebf-0ae9-41b0-b368-11a367dcb554-10826299.jpg)  
        
    
    -   Method Area(메소드 영역)  
        -   클래스 변수의 이름, 타입, 접근 제어자 등과 같은 클래스와 관련된 정보를 저장한다. 그 외에도 static 변수, 인터페이스 등이 저장된다.  
            
    
    -   Heap Area(힙 영역)  
        -   new를 통해 생성된 객체와 배열의 인스턴스를 저장하는 곳이다. 가비지 컬렉터는 힙 영역을 청소하며 메모리를 확보한다.  
            
    
    -   Stack Area(스택 영역)  
        -   메소드가 실행되면 스택 영역에 메소드에 대한 영역이 1개 생긴다. 이 영역에 지역변수, 매개변수, 리턴값 등이 저장된다.  
            
    
    -   PC register(PC 레지스터)  
        -   현재 쓰레드가 실행되는 부분의 주소와 명령을 저장한다.(CPU의 레지스터와 다르다.)  
            
    
    -   Native Method Stack(네이티브 메소드 스택)  
        -   자바 외의 언어(C, C++ 등)로 작성된 코드를 위한 메모리 영역이다. JNI를 통해 사용된다.  
            
-   가비지 컬렉터  
    -   가비지 컬렉터란?  
        -   '더이상 참조되지 않는 메모리'인 가비지를 청소해주는 JVM의 실행 엔진의 한 요소.  
            
-   제네릭  
    -   제네릭이란?  
        -   체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안전성을 높이고 형변환의 번거로움을 덜어주어 자연스럽게 코드도 더 간결