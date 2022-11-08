## MVC 패턴에 대해서  
- [ ] 어떻게 동작하는거야?
    - 간단하게, Controller에서 모든 것을 다함. 이후 뷰에 넘겨주기
    - 그림으로 이해하기
    - ![](https://api.transno.com/v3/document_image/ede06479-9385-44fb-b76a-cb1486c21a74-10826299.jpg)  
        동적으로 실행되긴 위해서는 비즈니스 로직을 실행해서 데이터를 만들어 전달해 줘야되는데 전달하는 매개체는 Model. Model이 뷰에 데이터를 넣어줘 렌더링하면 응답완료.
    - 매커니즘
        -   1. 클라이언트가 **컨트롤 로직**을 호출해  
        -   2. 컨트롤 로직이 http 스펙, 파라미터를 확인후 **서비스를 호출**해 넘겨준다.  
        -   3. 서비스가 **비즈니스 로직** 수행  
        -   4. 결과값을 모델이 받아서 **저장하고 뷰**에 넘긴다.  
        -   5. 뷰는 그 값을 가져다가 쓴다.  
    - 코드로 보기  
        -   servletmvc.MvcMemberFormServlet : 회원 등록 폼 - 컨트롤러  
        -   servletmvc.MvcMemberSaveServlet : 회원 저장 - 컨트롤러  
        -   servletmvc.MvcMemberListServlet : 회원 목록 조회 - 컨트롤러  
    - 문제점?  
        -   중복 http가 많이 반복되어 **유지보수 어려워**, **공통처리 X** => frontcontroller      
- [ ] mvc 각각의 역할   
    -   Model  
        -   뷰에 출력할 데이터를 담는다.  
        -   뷰에 필요한 데이터를 모두 모델에 저장하여 전달한다.  
        -   뷰는 비즈니스 로직이나 데이터 접근에 대해 몰라도 된다.  
    -   View  
        -   모델에 담긴 데이터를 사용해 화면을 그리는 일에 집중한다.  
    -   Controller  
        -   1)HTTP 요청을 받아서 2) 파라미터를 검증한다.  
        -   3)비즈니스 로직을 실행한다.  
        -   4)뷰에 전달할 결과 데이터를 조회해 모델에 담는다.  
    -   실제 코드 비교![](https://api.transno.com/v3/document_image/9855e838-59e8-440b-8586-f1c220814cc7-10826299.jpg)  
		Controller가 너무 많은 일을 하는데..
		- /WEB-INF  
			-   이 경로안에 JSP가 있으면 외부에서 직접 JSP를 호출할 수 없다. 우리가 기대하는 것은 항상 컨트롤러를 통해서 JSP를 호출하는 것  
		- redirect vs forward  
			-   redirect  
				-   실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다.  
			-   forward  
				-   포워드는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.                
- [ ] mvc 패턴의 한계  
    -   공통 처리가 어렵다는 문제(포워드/ViewPath 중복, 사용하지 않는 코드 있음)  

## 프론트 컨트롤러 패턴 만들기
-   [프론트 컨트롤러 패턴 링크](https://dodeon.gitbook.io/study/kimyounghan-spring-mvc/04-create-mvc-framework/front-controller-pattern)  
- [ ]   공통처리를 위해서 어떻게 바뀌는 거야?  
	-   **프론트 컨트롤러(수문장)의 유무**![](https://api.transno.com/v3/document_image/0aa255e9-6149-4381-9c08-60ae6b5e2cf9-10826299.jpg)  
		프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음. **프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출**.
	
	-   중간에 appconfig처럼 제어하는 인터페이스 생성댐![](https://api.transno.com/v3/document_image/a65f5829-c0c6-4aaa-b264-38ff60edd982-10826299.jpg)  
		인터페이스를 구현하는 컨트롤러를 만들고 기존 서블릿 로직을 그대로 복붙한다.(다형성 활용)
- [ ]   [어떻게 설계할꺼야??](https://dodeon.gitbook.io/study/kimyounghan-spring-mvc/04-create-mvc-framework/front-controller-pattern)  
	-   1. <이름, 각 클래스> 생성 후 hashmap에 보관  
	-   2. 이제 이곳에서 url 받고 , 다형성을 활용해 선택 함.  
	-   3. 해당 컨트롤러로 호출하기(이전에 했던 행동 2~5을 여기서 함.)  
	-   4. 뷰 출력  
- [ ]   코드로 보기(frontcontroller.v1)  
	-   ControllerV1 : 일관성을 위한 컨트롤러 인터페이스를 도입  
	-   컨트롤러 : 이전과 동일함  
		-   MemberFormControllerV1 - 회원 등록 컨트롤러  
		-   MemberSaveControllerV1 - 회원 저장 컨트롤러  
		-   MemberListControllerV1 - 회원 목록 컨트롤러  
	-   FrontControllerServletV1 : 공통처리(모아놓고 선택하기)를 위한 프론트 컨트롤러  
- [ ]   문제점?  
	-   모든 컨트롤러에서 **뷰로 이동하는 부분에 중복** => 메소드 만들어야겠다 + 상태도 같이 있네? 클래스로 구현해야겠다.  
- [ ]   [Crocus](https://www.crocus.co.kr/1406)(디스패처란 무엇인가?)  
	- 그림으로 이해하기![[Pasted image 20221011230125.png]]
	- 준비상태(Ready)에서 실행상태(Running)로 상태전이되는 과정을 의미함

## View 분리하기
[View 분리 링크](https://dodeon.gitbook.io/study/kimyounghan-spring-mvc/04-create-mvc-framework/view)
- [ ]   그래서 중복 문제를 어떻게 해결할거야?
	- 그림으로 이해하기![](https://api.transno.com/v3/document_image/e0b73636-38e9-42ab-8e3e-9b16fe715c14-10826299.jpg)  
	- 매커니즘
		-   1. 객체를 만들어서 상태에 이동경로 저장, 메소드에 render 저장  
		-   2. 그래서 이제는 Myview객체만 반환하면 코드 단순화  
			-   myview의 역할  
				-   1. URI 저장  
				-   2. 렌더링  			
- [ ]   코드로 보기  
	-   MyView : 각 Controller에 있는 뷰 이동하는 부분을 공통적으로 처리하기 위한 클래스  
- [ ]   실제 코드 비교  
	-   내가 무엇을 잘 모르는가...![](https://api.transno.com/v3/document_image/60016f4b-b9d6-4cfc-b298-17fa214d7c2e-10826299.jpg)  
		-   1) 타입 ControllerV2 받는거 알겠어 ㅇㅋ, 다형성을 이용해 Form, Save, List를 부모 타입으로 받는다. 그런데? 객체를 생성해야되는데 뭔 이상한 문자열이 들어갈까... controllerMap에 어떻게 문자가 들어갔나봐야함. ["/front-controller/v2/members/new-form", new MemberFormControllerV2(), [URL, 객체생성].. ]. 즉 문자열이 아닌 new 해당 객체생성들어감.  
		-   2) MyView에 상태는 뷰 URL(/WEB-INF/~ 형태) 가지고 있고, 함수는 렌더링할수 있는 함수가 들어있음  
- [ ]   문제점?  
	-   **HttpServletRequest/Response**가 꼭 필요하지 않다.  
		-   why? 요청 파라미터 정보는 Map으로 넘기면 이 서블릿 기술을 몰라도 동작할 수 있다. Model은 HttpServletRequest를 사용하는 대신, 별도의 Modelview 객체를 만들어 반환함.  
		-   그러면? 요청 파라미터는 어떻게 받지??  
	-   뷰의 중복 이름 제거 => 단순화  

##  Model 분리하기
- [ ]   HttpServlet을 어떻게 제거해서 해결할까?
	- ![](https://api.transno.com/v3/document_image/5df0a10c-e6e4-4d34-8083-7987a88f226f-10826299.jpg)  
	-   1. 프론트컨트롤에서 대신 요청 파라미터를 받아서, paramap에 저장  
		-   요청과 응답이 필요 없는 경우는?? 상관없어 어쩌피 파라미터를 안받으니깐  
	-   2. 그전에 고른 Controller에 paramap을 넣으면 Modleview를 반환.  
		-   Modelview의 역할은 뭐야?  
			-   View 이름 -> why? 이름은 단순화 해서 다시 붙여야함  
			-   View를 렌더링할 때 필요한 Model 객체(비즈니스 로직 처리한데이터)  
	-   3. 단순화한 view이름 이어서 URI 만들기      
	-   4. 렌더링  
- [ ]   문제점?  
	-   항상 ModelView를 생성하고 반환해야 해서 번거롭다. => ModelView 대신 ViewName(String)만 반환  

## 단순하고 실용적인 컨트롤러  만들기
- [ ]  어떻게 ModelView 대신 ViewName(String)만 반환할까?
	- ![](https://api.transno.com/v3/document_image/2f530de0-5d80-40b0-940f-348f93d9c500-10826299.jpg)  
	-   1. Frontcontroller에 hasmap<String, object> model 생성
		-   첫번째는 URI를 간단히 만든 이름  
		-   두번째는 Myview모델 객체(비즈니스 로직 데이터)를 저장  
			-   ?? 그러면 <String, modelview> 이거 아냐?  
	-   2. 이전과 동일 		
- [ ]   문제점?  
	-   프론트 컨트롤러에는 이미 버전으로 타입이 박혀있기 때문에 **다른 버전을 사용**할 수가 없다. conrtrollerv4말고도 controllerv3도 같이 쓰고 싶어  

## 유연한 컨트롤러 만들기
- [ ]   어떻게 다른 버전도 함께 사용할 수 있게 할까?
	- ![](https://api.transno.com/v3/document_image/ea2c9819-c3d1-4c6a-abe9-350c475d6aa5-10826299.jpg)  
	-   1. 이제 핸들러 매핑정보만 가져오기(initHandlerMappingMap 중 변환할거 가져오는 거임)  
	-   2. 가져온 핸들러에 맞는 어댑터 찾기(MyHandlerAdapter에서)  
	-   3. 핸들러를 어댑터에 연결하여 변환하기(MyHandlerAdapter에서)  
	-   4. 같은 메서드에서 비즈니스 로직 처리후, Myview변환하기