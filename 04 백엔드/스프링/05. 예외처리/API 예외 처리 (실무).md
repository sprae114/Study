
#JSON예외처리, #API예외처리

#인프런MVC_2편

코드 위치 : inflearn-mvc2/exception

----
#인프런MVC_2편07(page. 264)

## 예외처리 어떤 것을 선택해야할까?✔
#### HTML 페이지 오류
스프링 부트가 기본으로 제공하는` BasicErrorController`를 활용하여 처리함. 

#### API 오류
1) `~ControllerAdvice` 클래스를 만들어서 예외 로직을 구현
2) `@ExceptionHandler`을 사용해서 예외로직 작성


##  API 예외 처리의 어려운점은? ✔
- `HandlerExceptionResolver`를 구현할때를 생각해보면, **ModelAndView를 반환**했다. 이는 API응답에서는 **불필요**하다.
![[Pasted image 20230125183001.png]]
- `HandlerExceptionResolver`를 상속하여 구현할때를 생각해보면 **response에 직접 응답 데이터를 다 처리**하기에 너무 불편했다.


## ExceptionHandlerExceptionResolver란?✔
`@ExceptionHandler` 어노테이션이 붙은 메소드를 찾아 **해당 메소드가 처리할 수 있는 예외를 전달하는 역할**을 합니다. 지정한 예외 또는 그 예외의 자식 클래스는 모두 잡을 수 있다.

```java
@Controller
public class MyController {
    // ...
    @ExceptionHandler(IOException.class)
    public String handleIOException(IOException ex, HttpServletRequest request) {
        // 예외 처리 로직
        return "myErrorView";
    }
}
```


## 예외처리 매커니즘 ✔
#### 그림으로 보는 매커니즘
```java
1. WAS -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(예외발생)
2. @ExceptionHandler으로 예외처리 (정상흐름/오류변환 둘다 가능)
3. 컨트롤러 -> View
```
![[Pasted image 20231130005638.png]]


#### 코드로 보는 매커니즘
```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(IllegalArgumentException.class)
public ErrorResult illegalExHandler(IllegalArgumentException e){
    log.error("[exceptionHandler] ex", e);
    return new ErrorResult("BAD", e.getMessage());
}
```
1) 컨트롤러를 호출한 결과 **`IllegalArgumentException` 예외**가 컨트롤러 밖으로 전달된다.
2) 예외가 발생했으니 가장 우선순위가 높은 **`ExceptionHandlerExceptionResolver`가 있으므로**, 해당 컨트롤러에서 IllegalArgumentException을 처리할 수 있는 **`@ExceptionHandler` 가 있는지 확인**한다.
3) **`illegalExHandle()` 를 실행**한다. `@RestController` 이므로 `illegalExHandle()` 에도 **@ResponseBody 가 적용**된다. 따라서 HTTP 컨버터가 사용되고, 응답이 다음과 같은 JSON으로 반환된다.
5) **@ResponseStatus(HttpStatus.BAD_REQUEST) 를 지정**했으므로 **HTTP 상태 코드 400으로 응답**한다.


## (코드) `@ExceptionHandler` 예외 처리✔
[Exceptions :: Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-exceptionhandler.html)
`@ExceptionHandler`의 이용은 `ExceptionHandlerExceptionResolver`를 사용한다는 것을 의미

#### 기본 구조
▶ API 응답 객체
예외가 발생했을때 **JSON을 담을 객체**
```java
@Data
@AllArgsConstructor
public class ErrorResult {
    private String code;
    private String message;
}
```

▶ 컨트롤러
```java
@Slf4j
@RestController
public class ApiExceptionV2Controller {

	@ExceptionHandler
	public ErrorResult ExHandler(Exception e){
		...
	}
	
    @GetMapping("/api2/members/{id}")
    public MemberDto getMember(@PathVariable String id){
		if (id.equals("status")){  
		    throw new StatusException("잘못된 상태");  
		}  
		  
		if (id.equals("ex")) {  
		    throw new RuntimeException("잘못된 사용자");  
		}  
		
		if (id.equals("bad")) {  
		    throw new IllegalArgumentException("잘못된 입력 값");  
		}  
		
		if (id.equals("user-ex")) {  
		    throw new UserException("사용자 오류");  
		}
		
		return new MemberDto(id, "hello " + id);
	}

	...
}
```


#### 정상 흐름
▶요청 : `http://localhost:8083/api2/members/human`
▶응답결과
![[Pasted image 20230126015134.png]]
정상적인 응답 처리


#### @ExceptionHandler만 있는경우
▶요청 : `http://localhost:8083/api2/members/status`
▶작성 코드
```java
@ExceptionHandler(StatusException.class)  
public ErrorResult statusHandler(StatusException e) {  
    log.error("[exceptionHandler] ex", e);  
    return new ErrorResult("only", e.getMessage());  
}
```
▶응답 결과
![[Pasted image 20231130191718.png]]
**오류를 반환했지만, HTTP상태코드 == 200 으로 정상인 상태**가 나오는 문제가 발생함.


#### @ResponseStatus가 추가된 경우
▶요청
`http://localhost:8083/api2/members/bad`
▶작성코드
```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(IllegalArgumentException.class) 
public ErrorResult illegalExHandler(IllegalArgumentException e){
	log.error("[exceptionHandler] ex", e);
	return new ErrorResult("BAD", e.getMessage());
}
```
▶응답 결과
![[Pasted image 20230126015013.png]]
해당 오류와 상태코드가 지정한대로 **정확하게 예외처리** 됐다.


#### ResponseEntity 반환
▶요청 : `http://localhost:8083/api2/members/user-ex`
▶작성코드
```java
@ExceptionHandler //파라미터 예외랑 같은 경우 생략가능
public ResponseEntity<ErrorResult> userExHandler(UserException e){
	log.error("[exceptionHandler] ex", e);
	ErrorResult errorResult = new ErrorResult("USER-EX", e.getMessage());
	return new ResponseEntity<ErrorResult>(errorResult, HttpStatus.BAD_REQUEST);
}
```
▶응답 결과
![[Pasted image 20230126015211.png]]
`ResponseEntity` 를 사용해서HTTP 응답 (오류 메시지 + HTTP 메시지)로 구성함.


#### 처리한적 없는 Exception
▶요청 : `http://localhost:8083/api2/members/ex` 
▶작성코드
```java
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)  
@ExceptionHandler  
public ErrorResult exHandler(Exception e) {  
    log.error("[exceptionHandler] ex", e);  
    return new ErrorResult("EX", "내부 오류");  
}
```
▶응답 결과
![[Pasted image 20230126015041.png]]
`IllegalArgumentException`예외 발생했지만, `ExControllerAdvice`에서 해당예외만을 처리하는 함수가 없다. 그래서 **가장 상위 `Exception`을 호출해서 예외처리**


## @ControllerAdvice란? ✔

**정상 코드와 예외 처리 코드가 분리해주는 역할**. `@ControllerAdvice` 또는 `@RestControllerAdvice` 를 사용하면 둘을 분리할 수 있다.

▶@ControllerAdvice
- @ControllerAdvice 는 대상으로 지정한 여러 컨트롤러에 **@ExceptionHandler** , @InitBinder **기능을 부여**해주는 역할을 한다. 대상을 지정하지 않으면 모든 컨트롤러에 적용된다. (글로벌 적용)
- @RestControllerAdvice 는 @ControllerAdvice 와 같고, @ResponseBody 가 추가되어 있다.


## 특정 컨트롤러만 예외처리하도록 처리하는 방법 ✔
[Controller Advice :: Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-advice.html)

▶애노테이션으로 범위 지정
```java
@ControllerAdvice(annotations = RestController.class) 
public class ExampleAdvice1 {}
```

▶패키지 별로 지정
```java
@ControllerAdvice("org.example.controllers") 
public class ExampleAdvice2 {}
```

▶여러 패키지 별로 지정
```java
@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
public class ExampleAdvice3 {}
```


## (코드) 예외코드와 정상코드 분리하기✔
#### ExControllerAdvice
예외 클래스를 만들어 **예외 분리**하기
```java
@Slf4j
@RestControllerAdvice
public class ExControllerAdvice {

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(IllegalArgumentException.class)
    public ErrorResult illegalExHandler(IllegalArgumentException e){
        ...
    }

    @ExceptionHandler
    public ResponseEntity<ErrorResult> userExHandler(UserException e){
	    ...
    }

    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ExceptionHandler
    public ErrorResult exHandler(Exception e){
	    ...
    }
}
```

#### ApiExceptionV3Controller
컨트롤러에는 **비즈니스 로직만** 옴
```java
@Slf4j  
@RestController  
public class ApiExceptionV3Controller {  
  
    @GetMapping("/api3/members/{id}")  
    public MemberDto getMember(@PathVariable("id") String id) {  
		  ...
    }  

	...
}
```
