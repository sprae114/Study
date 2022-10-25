## 로깅에 대해서 알아보자
- [ ]   로깅 라이브러리  
	-   slf4j  
		-   logback, log4j 등 다양한 라이브러리를 통합해 인터페이스로 제공하는 라이브러리  
	-   logback -> 실무에서 사용  
		-   실제 로깅 구현체  
- [ ]   로깅의장점  
	-   스레드 정보, 클래스 이름 같은 **부가 정보**를 볼 수 있다.  
	-   환경에 따라 **로그 레벨을 조절**할 수 있다.  
	-   성능도 System.out보다 좋다.  
- [ ]   로깅 레벨 설정  
	-   trace > debug > info > warn > error  
	-   보통 개발 서버는 debug, 운영 서버는 info로 출력한다.  
	-   로그 레벨 바꾸는 방법은?
		- ![](https://api.transno.com/v3/document_image/5b10b440-7219-466a-99c3-318890c89af7-10826299.jpg)  

## 요청 매핑에 대해서 알아보자
- [ ]   @ RequestMapping  
	-   배열을 통한 다중 설정 가능![](https://api.transno.com/v3/document_image/7494cc43-1dc1-4769-aeb8-bf3d497419f4-10826299.jpg)  
	-   메서드 속성은 HTTP 메서드를 지정하지 않으면 **뭘로 요청하든 무관하게 호출**된다.  
- [ ]   HTTP 메서드 매핑  
	-   **GET메서드로 요청 제한**함![](https://api.transno.com/v3/document_image/ae0c6973-75ab-441d-bdd3-26257d6e021e-10826299.jpg)  
	-   너무 길어 축약버전(애노테이션에 메소드 들어감)![](https://api.transno.com/v3/document_image/2cbc3720-ee13-4f6f-84ce-12f235abd02a-10826299.jpg)  
- [ ]   경로 변수(PathVariable)  
	-   다음과 같이 **리소스 경로에 식별자**를 넣어야함.  
	-   들어오는 데이터로 **경로를 지정**할 수 있을까?![](https://api.transno.com/v3/document_image/ec32fa69-3054-416d-a4be-21c1ab8f53eb-10826299.jpg)  
	-   다중 설정![](https://api.transno.com/v3/document_image/a2b0f44a-59f0-41a4-a54c-8945a54fe1a7-10826299.jpg)  
- [ ]   조건 매핑  
	-   파라미터 조건 매핑, 해더 조건 매핑 있는데 잘 안쓰임  
	-   HTTP 요청 Content-Type, consume![](https://api.transno.com/v3/document_image/d35f643f-0bce-462e-aff9-ea31fc4b8ba1-10826299.jpg)  
		-   html인지 등 타입에 따라 쓰려면 headers가 아니라 consumes를 사용  
	-   HTTP 요청 Accept, produce![](https://api.transno.com/v3/document_image/6b93613a-f3ef-4b96-bdae-a3e7dcd96346-10826299.jpg)  
		-   웹 브라우저 측에서 받아들일 수 있는 타입  

## HTTP 요청에서 기본, 헤더 조회하는 방법은?
- [ ]   코드로 보는 기본,헤더 조회방법
	- 코드![](https://api.transno.com/v3/document_image/654d6133-9b59-45c5-a3cb-547127bcc2ed-10826299.jpg)  
	-   결과![](https://api.transno.com/v3/document_image/4f847117-64d0-4cae-95d1-1fe645f52028-10826299.jpg)  
- [ ]   locale이란?
	-   locale정보(언어 우선순위 등..)  
- [ ]   MultiValueMap이란?
	-   MAP과 유사한데, 하나의 키에 여러 값을 받을 수 있다.  
	-   예) keyA=value1&keyA=value2![](https://api.transno.com/v3/document_image/a5f11562-7020-4590-a96b-09674f899f33-10826299.jpg)  
- [ ]   `@ CookieValue`  
	-   애노테이션을 이용하여 **쿠키**를 전달 받을 수 있다.  
	-   value 속성을 전달 받을 쿠키의 이름을 지정한다.  
	-   required 속성을 true로 지정 시, value 속성의 이름을 가진 쿠키가 존재하지 않을 시에 스프링 MVC는 익셉션을 발생시킨다.  

## HTTP 요청 파라미터 방법은?
- [ ]   요청 파라미터( 조회가 아니고 요청임)  
	-   쿼리 파라미터
		- ![](https://api.transno.com/v3/document_image/f5d3d0be-5074-4d43-8879-2e5e7d971a0f-10826299.jpg)  
		클라이언트가 요청할때는 HTTP 메소드로 조회해야되는거 아냐?? 근데 여기에는 메소드가 없는데..  
		URI와 HTTP요청의 차이는 뭐지.. 아 저 형식은 GET 방식이라고 생각하면 됌. 바디가 없으니깐  
	-   HTML Form
		- 코드
		- ![](https://api.transno.com/v3/document_image/12601544-9885-41ca-a001-a9ef0aa567a8-10826299.jpg)![](https://api.transno.com/v3/document_image/2ae2713c-6921-4cfe-9c47-679aeaf39622-10826299.jpg)![](https://api.transno.com/v3/document_image/c187cb6f-ff00-442e-9fa7-18aa190a33ec-10826299.jpg)  
		view를 통해서 html메시지를 만들어서 전송함. content-type을 보면 html form이라는 것을 알 수 있음.  
		​그러면 애는 파라미터를 어떻게 저장하는 걸까?? 자동으로 서블릿에 저장됌
- [ ]   조회하는 방법  
	-   @ RequestParam  
		-   (쿼리 파라미터 실제코드 변천사)![](https://api.transno.com/v3/document_image/4f912757-122e-48bf-8d30-8b43e9d5e4ef-10826299.jpg)![](https://api.transno.com/v3/document_image/c589ba84-5a21-4bc4-a3b5-03aaca33ba01-10826299.jpg)  
			HttpServletRequest/Response를 @ RequestParm애노테이션으로 간단히 나타냄  
			또한, 파라미터와 변수명이 동일하면 파라미터명 생략가능  
			@ ResponseBody를 통해 메시지 바디로 응답함.
	-   파라미터 값이 **비어있게** 하고 싶을때는 어떻게 해야하지?  
		-   required = false로 하면 빈문자로 통과 가능![](https://api.transno.com/v3/document_image/b2a2ebf4-5e67-4812-9adc-9e661553e86a-10826299.jpg)  
		-   주의점  
			-   필수값을 넣지 않으면 400 에러가 발생한다.  
			-   null이 아니라 ?username=처럼 빈 값이 들어오는 경우라면 ok가 떨어지므로 주의.![](https://api.transno.com/v3/document_image/d5523d5c-9b6c-40e9-b950-981d8a101163-10826299.jpg)  
	-   파라미터가 입력이 되지 않을 때, **기본값 적용**할려면?  
		-   defaultValue 에 기본값 넣기(requried = true는 말이 안되서 회색임)![](https://api.transno.com/v3/document_image/2fe3144a-46fd-48eb-9d14-6eb65cc688f4-10826299.jpg)  
	-   파라미터 값이 **두개 이상이라면**? 
		-   MultValueMap![](https://api.transno.com/v3/document_image/25adc6b9-9e0d-4ad7-89e1-cd47d9ddc32f-10826299.jpg)  
			Map과 MutiValueMap과 차이 비교해보기!!
	-  값을 **객체**에 담고 싶을때라면?
		-  `@ModelAttribute`   :  단순히 String, int를 저장하는게 아니고 **값을 객체에 담아서 저장**하고 싶을때 사용함.  
			-   객체만들기
			- ![](https://api.transno.com/v3/document_image/b41de83d-7e18-4239-a34b-37fc4bb902f5-10826299.jpg)  
			
			-   실제 코드![](https://api.transno.com/v3/document_image/44309aa2-aa1d-4806-9f68-abcc45387a9e-10826299.jpg)  
				이것을 쓰기 위해서는 객체의 변수 이름과 파라미터 이름을 동일하게 해야함
				-   생략 가능한 부분![](https://api.transno.com/v3/document_image/42c687e4-3365-4cf2-820c-e43c3590aecb-10826299.jpg)  

## HTTP 요청 메시지  방법은?
- [ ] HTTP 요청 파라미터와 뭐가 다르지?
	-   요청 파라미터와 다르게, **HTTP 메시지 바디**를 통해 데이터가 직접 데이터가 넘어오는 경우는@ RequestParam , @ ModelAttribute 를 사용할 수 없다.  
- [ ] 단순 텍스트  변천사(이해를 돕기 위함)
	- 코드![](https://api.transno.com/v3/document_image/6c9db12e-0a3e-4f10-9db2-19b944b9af7e-10826299.jpg)![](https://api.transno.com/v3/document_image/6ca6f28b-bec7-4e38-a5c3-9c1247c579d2-10826299.jpg)  
		초기에는 request에서 getInputStream로 바디데이터를 읽는다. 이후, 스트림은 항상 바이트코드 변환해야함 그래서 copyToString으로 함.  
		이것을 단순화 하는게 @ RequestBody를 사용하면 바디 바디 정보를 편리하게 조회함.(view 사용X) 단 2번째 그림에 @ ResponseBody(HttpBody에 메시지 직접입력)과 헷갈리지 말기
- [ ] @RequestBody/ @ResponseBody의 차이  
	-   @ RequestBody  
		-   HTTP 메시지 바디 정보를 편리하게 조회할 수 있다. 참고로 헤더 정보가 필요하다면 HttpEntity 를 사용하거나 @ RequestHeader 를 사용하면 된다.  
	-   @ ResponseBody  
		-   응답 결과를 HTTP 메시지 바디에 직접 담아서 전달할 수 있다.  		
	- ![](https://api.transno.com/v3/document_image/adff8b36-c66a-49be-98bb-2c368c88e87c-10826299.jpg)	
- [ ] JSON  
	-   JSON 변천사![](https://api.transno.com/v3/document_image/dcd5e0b7-bdbd-402d-a9d3-c23ba602381b-10826299.jpg)![](https://api.transno.com/v3/document_image/a8da7b5c-2355-4446-be86-8b310278a091-10826299.jpg)  
		이전과 동일하게 바이트코드까지 변환하는데, 이후 해당 바이트 코드를 objectMapper로 통해 JSON으로 변환함.  
		HTTP 요청시에 content-type이 application/json인지 꼭! 확인해야 한다. 그래야 JSON을 처리할 수 있는 HTTP 메시지 컨버터가 실행

## HTTP 응답하는 방법은?
- [ ]  정적 리소스  
	-   예) 웹 브라우저에 정적인 HTML, css, js을 제공할 때는, 정적 리소스를 사용  
	-   다음 디렉토리(/static)에 리소스를 넣어두면 스프링 부트가 정적 리소스로 서비스를 제공한다.  
- [ ]   뷰 템플릿  
	-   예) 웹 브라우저에 동적인 HTML을 제공할 때는 뷰 템플릿을 사용  
	-   다음 디렉토리(/templates)에 리소스를 넣어두면 뷰 템플릿을 거쳐서 HTML이 생성되고, 뷰가 응답을 만들어서 전달함.  
	-   어떻게 동작하는 거야?  
		-   1. html form에서 메시지 전송함![](https://api.transno.com/v3/document_image/ccf69445-53bd-48ee-8425-5292d591c13e-10826299.jpg)  
			URI는 html이 있는 위치를 말하고
		-   2. 전송을 누르면 action 위치로 URI이동 ![](https://api.transno.com/v3/document_image/5b24b0f9-6681-402f-a359-8eb895940455-10826299.jpg)  
		-   3. 이후, Controller에서 해당 URI 찾은후 메소드(Post) 실행![](https://api.transno.com/v3/document_image/b9dc8919-2a04-42dc-ad76-fb1bad9fd527-10826299.jpg)  
		-   3-2 이후, Controller에서 해당 URI 찾은후 메소드(Post) 실행후, veiw 렌더링하는 경우![](https://api.transno.com/v3/document_image/33ddd4fe-f07f-42a0-bf6f-470bb9d13f43-10826299.jpg)  
		-   4-2. 해당 view 렌더링 ${data}부분에 "hello!" 넣기![](https://api.transno.com/v3/document_image/737a9e37-79d2-4ca7-8ef1-3e181beba915-10826299.jpg)  
- [ ]   HTTP API, 메시지 바디에 직접 입력  
	-   HTTP API를 제공하는 경우에는 HTML이 아니라 데이터를 전달해야 하므로, HTTP 메시지 바디에 JSON 같은 형식으로 데이터를 실어 보낸다.  
	-   @RestController  
		- @RestController = @Controller + 모든 메서드에 @ResponseBody 붙이기  
		- 예시
			- 단순 텍스트![](https://api.transno.com/v3/document_image/c2d50204-0376-498c-b0e9-175c302de125-10826299.jpg)  
				첫번째는 리턴없이 서블릿응답 객체를 http바디에 직접 보내는 경우  
				두번째는 Http정보를 함께 보내는 경우  
				세번째는 @ ResponseBody를 이용해서 http바디에 직접 보내는 경우
		
			-   JSON![](https://api.transno.com/v3/document_image/8562682f-0d33-4a5e-8689-6f26be9e9d7c-10826299.jpg)  
				첫번째는 ResponseEntity에 HttpStatus를 담아 보낸다.  
				두번째는 위에서 알아본 축약버전이다. 하지만 http상태를 붙이고 싶은 경우 @ ResponseStatus를 이용해서