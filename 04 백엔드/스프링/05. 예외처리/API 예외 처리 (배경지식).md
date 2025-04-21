
#JSON예외처리, #API예외처리

#인프런MVC_2편

코드 위치 : inflearn-mvc2/exception

----
[스프링의 다양한 예외 처리 방법(ExceptionHandler, ControllerAdvice 등) 완벽하게 이해](https://mangkyu.tistory.com/204)
#인프런MVC_2편07(page. 264)

# API의 예외 처리 시작
## 목표
1) view 반환 -> Json 반환
2) 5xx 오류 -> 4xx 오류로 오류 변환
3) 같은 오류여도 컨트롤러에 따라 오류내용 달라지게


## API 예외처리는 기존 예외처리하고 무엇이 다를까?✔
예외처리를 view를 반환하여 화면으로 하는것이 아니라 API 방식은 **각 오류 상황에 맞는 오류 응답 스펙을 정하고, JSON**으로 데이터를 내려주어야 한다.
![[Pasted image 20231129181154.png]]


# 직접 코드 작성해 JSON반환
## (코드) 기존 예외처리하면 생기는 문제점은?
문제 상황 : 정상 호출 JSON 반환 + **오류 호출 View 반환**

▶WebServerCustomizer
WebServerCustomizer를 만들어서 **직접 오류를 처리하는 매커니즈 사용**함.(예외처리와 오류페이지 내용 참조)

▶ API 컨트롤러
ex라는 사용자가 들어오면 예외발생함.
```java
@Slf4j
@RestController
public class ApiExceptionController {

    @GetMapping("/api/members/{id}")
    public MemberDto getMember(@PathVariable String id){
        if(id.equals("ex")){
            throw new RuntimeException("잘못된 사용자"); //View로 반환
        }

        return new MemberDto(id, "hello " + id); // JSON로 반환
    }

    @Data
    @AllArgsConstructor
    static class MemberDto{
        private String memberId;
        private String name;
    }
}
```

▶ 결과
- 정상호출
```json
{
	"memberId": "spring", 
	"name": "hello spring"
}
```

- 런타임 에러(사용자가 ex)에서는 view로 반환
![[Pasted image 20230125160907.png]]


## (코드) 해결책 -> 이해만
해결 상황 : 정상 호출 JSON 반환 + **오류 호출 JSON 반환**

▶WebServerCustomizer
WebServerCustomizer를 만들어서 직접 오류를 처리하는 매커니즈 사용(예외처리와 오류페이지 내용 참조)

▶ Controller
오류를 **직접 코드 작성해 Json으로 변환**함.
```java
public class ErrorPageController {  

	...
	
    @RequestMapping(value = "/error-page/500", 
	    produces = MediaType.APPLICATION_JSON_VALUE)  
    public ResponseEntity<Map<String, Object>> errorPage500Api(
	    HttpServletRequest request, 
	    HttpServletResponse response) {  
  
        log.info(">> API errorPage 500");  
  
        Map<String, Object> result = new HashMap<>();  
        Exception ex = (Exception) request.getAttribute(ERROR_EXCEPTION);  
        result.put("status", request.getAttribute(ERROR_STATUS_CODE));  //오류상태 넣기
        result.put("message", ex.getMessage());  //오류 메시지 넣기
  
        Integer statusCode = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);  
        return new ResponseEntity<>(result, HttpStatus.valueOf(statusCode));  
    }
```
 `produces = MediaType.APPLICATION_JSON_VALUE` : 클라이언트가 요청하는 HTTP Header의 Accept 의 값이 application/json일 때 해당 메서드가 호출된다는 것이다.

▶ 요청
요청 URL : `http://localhost:8081/api/members/ex`
![[Pasted image 20230125165108.png]]

▶결과
```json
{
    "message": "잘못된 사용자",
    "status": 500
}
```

# 스프링이 기본으로 제공하는 오류 처리
## 스프링 부트가 제공하는 기본 오류 처리
스프링 부트가 제공하는 **BasicErrorController** 코드로 간편하게 예외를 처리

▶BasicErrorController 인터페이스 구성
`/error` 동일한 경로를 요청함.
```java
@Controller  
@RequestMapping("${server.error.path:${error.path:/error}}")  
public class BasicErrorController extends AbstractErrorController {
	@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {...}
	
	@RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {...}
}
```
- `errorHtml()` : `produces = MediaType.TEXT_HTML_VALUE` 이다. 따라서 클라이언트 요청의 **Accept가 text/html 인 경우에만 view 반환함.**
- `error()` : **그외 경우**에 호출되고 ResponseEntity 로 HTTP Body에 **JSON 데이터를 반환**함.


## (코드) 스프링 부트가 제공하는 기본 오류 처리
▶ API 컨트롤러
ex라는 사용자가 들어오면 예외발생함.
```java
@Slf4j
@RestController
public class ApiExceptionController {

    @GetMapping("/api/members/{id}")
    public MemberDto getMember(@PathVariable String id){
        if(id.equals("ex")){
            throw new RuntimeException("잘못된 사용자");
        }

        return new MemberDto(id, "hello " + id);
    }

    @Data
    @AllArgsConstructor
    static class MemberDto{
        private String memberId;
        private String name;
    }
}
```

▶ `BasicErrorController`로 예외처리 결과
BasicErrorController가 **자동으로 JSON 처리**해줌.


## BasicErrorController처리의 한계점은?✔
`BasicErrorController` **편리한 오류 HTML 페이지를 제공**하고 확장하면 JSON 메시지도 변경할 수 있다. 

그런데 **API 오류 처리는 API 마다, 각각의 컨트롤러나 예외마다 서로 다른 응답 결과를 출력**해야 하는 경우가 많다. 

예를 들어서, **같은 오류가 발생해도 회원이냐 상품이냐에 따라서 관련된 API 예외 내용이 달라질 수 있다.**

```json
{
    "status": 404,
    "error": "Not Found",
    "message": "해당 사용자를 찾을 수 없습니다."
}
```

```json
{
    "status": 404,
    "error": "Not Found",
    "message": "해당 상품을 찾을 수 없습니다."
}
```


# ExceptionResolver
## ExceptionResolver란?✔
`ExceptionResolver`(=`HandlerExceptionResolver`) 이름 그대로 예외를 해결하는 역할을 함. **기존 예외를 다른 예외로 교체하거나 예외흐름을 정상흐름으로 처리하는 용도**로 사용함.

▶ 인터페이스 구성
```java
public interface HandlerExceptionResolver {
	ModelAndView resolveException(
	HttpServletRequest request, 
	HttpServletResponse response, 
	Object handler, Exception ex);
}
```
- `handler` : 핸들러(컨트롤러) 정보 
- `Exception ex` : 핸들러(컨트롤러)에서 발생한 발생한 예외


## ExceptionResolver의 매커니즘은?✔
▶**기존 예외로 다른 예외로 교체하는 경우** 
예외(ex. 5xx)가 잡아서 다른 예외(ex. 400)로 바꿔줌.
```java
1. WAS -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(5xx 예외발생)
2. WAS(여기까지 전파) <- 필터 <- 서블릿 <- 인터셉터 <- 컨트롤러(ExceptionResolver 처리 후 예외 던지기)
3. WAS `/error-page/400` 다시 요청 -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(/error- page/400) -> View
```
![[Pasted image 20231130005721.png]]

![[Pasted image 20230125182905.png]]

▶ **정상흐름으로 바꿔주기**
```java
1. WAS -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(예외발생)
2. 컨트롤러(ExceptionResolver가 정상흐름 변환) -> View
```
![[Pasted image 20231130005721.png]]
**Exception을 처리해서 정상 흐름으로 변경**함. (단, postHandler는 호출 X)
![[Pasted image 20230125183001.png]]


## (코드) 5xx오류 -> 4xx오류
이해만

▶ Controller
- ApiExceptionController
```java
@GetMapping("/api/members/{id}")
public MemberDto getMember(@PathVariable("id") String id) {  
  
    if (id.equals("ex")) {  
        throw new RuntimeException("잘못된 사용자");  
    }  
    if (id.equals("bad")) {  
        throw new IllegalArgumentException("잘못된 입력 값");  
    }  
  
    return new MemberDto(id, "hello " + id);  
}
```


▶ ExceptionResolver
`IllegalArgumentException`에서 발생한 **500오류 -> 400오류로 바꾸고 싶음.**
```java
@Slf4j
public class MyHandlerExceptionResolver implements HandlerExceptionResolver {  
  
    @Override  
    public ModelAndView resolveException(HttpServletRequest request, 
	    HttpServletResponse response, Object handler, Exception ex) {  
  
        log.info("call resolver", ex);  
  
        try {  
            if (ex instanceof IllegalArgumentException) {  
                log.info("IllegalArgumentException resolver to 400");  
                response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage());  
                return new ModelAndView();  
            }  
  
        } catch (IOException e) {  
            log.error("resolver ex", e);  
        }  
  
        return null;  
    }  
}
```
- `response.sendError()` : **기존 오류를 다른 오류로 바꿔주는 역할**을 함. WAS가 `dispatcherType == ERROR `형식으로 다시 내부적인 요청을 하게 됌. (이것을 BasicErrorController가 처리)

▶ 반환 값에 따른 동작 방식
- `new ModelAndView() ` : **Exception을 처리후, 정상 흐름**
- `ModelAndView 지정` : **View , Model 등의 정보를 지정해서 반환하면 뷰를 렌더링** 
- `null` : null 을 반환하면, **다음 ExceptionResolver 를 찾아서 실행**한다. 만약 처리할 수가 없으면 예외 처리가 안되고, **기존에 발생한 예외**를 서블릿 밖으로 던진다.

▶ WebConfig
해당 **핸들러 등록**하기 위함.
```java
@Component
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        resolvers.add(new MyHandlerExceptionResolver());
    }
}
```

▶결과
- 적용전
![[Pasted image 20230125184303.png]]

- 적용후
![[Pasted image 20230125184641.png]]


## (코드) 예외 -> 정상흐름
이해만

▶Exception
오류 만들기
```java
public class UserException extends RuntimeException {  
  
    public UserException() {  
        super();  
    }  
  
    public UserException(String message) {  
        super(message);  
    }  
  
    public UserException(String message, Throwable cause) {  
        super(message, cause);  
    }  
  
    public UserException(Throwable cause) {  
        super(cause);  
    }  
  
    protected UserException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {  
        super(message, cause, enableSuppression, writableStackTrace);  
    }  
}
```

▶ Controller
user-ex일때 오류 처리
```java
@GetMapping("/api/members/{id}")
public MemberDto getMember(@PathVariable("id") String id) {  
  
    ... 
    
    if (id.equals("user-ex")) {  
    throw new UserException("사용자 오류");  
	}
  
    return new MemberDto(id, "hello " + id);  
}
```

▶ ExceptionResolver
Exception을 처리해서 **정상 흐름으로 변경하기 위한 목적**.
```java
@Slf4j  
public class UserHandlerExceptionResolver implements HandlerExceptionResolver {  
  
    private final ObjectMapper objectMapper = new ObjectMapper();  
  
    @Override  
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, 
									    Object handler, Exception ex) {  
  
        try {  
            if (ex instanceof UserException) {  
                log.info("UserException resolver to 400");  
                String acceptHeader = request.getHeader("accept");  
                response.setStatus(HttpServletResponse.SC_BAD_REQUEST);  
  
                if ("application/json".equals(acceptHeader)) {  
                    Map<String, Object> errorResult = new HashMap<>();  
                    errorResult.put("ex", ex.getClass());  
                    errorResult.put("message", ex.getMessage());  
                    String result = objectMapper.writeValueAsString(errorResult);  
  
                    response.setContentType("application/json");  
                    response.setCharacterEncoding("utf-8");  
                    response.getWriter().write(result);  
                    return new ModelAndView();  
                } else {  
                    // TEXT/HTML  
                    return new ModelAndView("error/500");  
                }  
            }  
        } catch (IOException e) {  
            log.error("resolver ex", e);  
        }  
  
        return null;  
    }  
}
```
- `response.getWriter().write(result);` : 예외를 Json으로 처리해버린다. 따라서 예외가 발생해도 서블릿 컨테이너까지 예외가 전달되지 않고, 스프링 MVC에서 예외 처리는 끝이난다. **때문에 WAS 입장에서는 정상처리되었다고 생각**한다.

▶ WebConfig
 `UserHandlerExceptionResolver` 등록하기 위함.
```java
@Component
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        resolvers.add(new MyHandlerExceptionResolver());
        resolvers.add(new UserHandlerExceptionResolver());
    }
}
```

▶결과
![[Pasted image 20230130220738.png]]

## ExceptionResolver의 한계점
`ExceptionResolver`를 사용하면 예외가 발생해도 서블릿 컨테이너까지 예외가 전달되지 않고, **스프링 MVC에서 예외 처리는 끝이난다.**
하지만, `ExceptionResolver`를 **직접 구현하려고 하니 상당히 복잡**하다. 


# @ExceptionResolver
## 스프링이 제공하는 3가지 ExceptionResolver

1. `ExceptionHandlerExceptionResolver` : **@ExceptionHandler** 를 처리한다.
2. `ResponseStatusExceptionResolver` : **HTTP 상태코드를 지정**해줄수 있다.
3. `DefaultHandlerExceptionResolver` : 우선 순위가 가장 낮다. **스프링 내부 기본 예외를 처리**한다


## ExceptionHandlerExceptionResolver란?
사용할 API 지식에서 확인하자.


## ResponseStatusExceptionResolver란?
**애노테이션을 활용하여 HTTP 상태코드를 지정해 예외를 처리**함. 하지만 정상흐름으로 바꿔주는 오류 X

▶ 클래스에서 사용하는 방식
**`@ResponseStatus`로 HTTP 상태 코드를 변경**해준다
```java
@ResponseStatus(code = HttpStatus.BAD_REQUEST, reason = "잘못된 요청 오류") 
public class BadRequestException extends RuntimeException {
}
```
`BadRequestException` 예외가 컨트롤러 `@ResponseStatus`를 확인해서 오류 코드를 `HttpStatus.BAD_REQUEST (400)`으로 변경하고, 메시지도 담는다.
하지만, 애노테이션을 직접 넣어야 하기 때문에 코드를 수정할 수 없는 라이브러리의 예외 코드 같은 곳에는 적용할 수 없다

▶ 메소드에서 사용하는 방식
```java
@GetMapping("/api/response-status-ex2")
public String responseStatusEx2() {
    throw new ResponseStatusException(HttpStatus.NOT_FOUND, "error.bad", new IllegalArgumentException());
}
```
**사용자가 원하는대로 설정하여 Exception을 발생**시킬수가 있다.
 

## ResponseStatusExceptionResolver 코드 파헤치기
```java
protected ModelAndView applyStatusAndReason(int statusCode, @Nullable String reason, HttpServletResponse response) 
    throws IOException {

    if (!StringUtils.hasLength(reason)) {
        response.sendError(statusCode);
    }
    else {
        String resolvedReason = (this.messageSource != null ?
                this.messageSource.getMessage(reason, null, reason, LocaleContextHolder.getLocale()) :
                reason);
        response.sendError(statusCode, resolvedReason);
    }
    return new ModelAndView();
}
```
 내부적으로는 **`response.sendError(statusCode, resolvedReason)`** 을 호출함. 그래서 **WAS에서 다시 오류 페이지**( /error )를 내부 요청한다.


## DefaultHandlerExceptionResolver란?
**스프링 기본으로 제공하는 예외 처리기**입니다. 컨트롤러에서 발생하는 특정 유형의 예외를 캐치하고, 적절한 HTTP 응답 상태 코드를 설정하는 역할을 합니다.

▶Controller
파라미터가 정상인 경우와 아닌 경우를 비교하기 위한 함수
```java
@GetMapping("/api/default-handler-ex")
public String defaultException(@RequestParam Integer data) {
    return "ok";
}
```

▶ 결과
- 정상호출
![[Pasted image 20230126003701.png]]

- 바인딩 시점 타입오류
![[Pasted image 20230126003630.png]]


## DefaultHandlerExceptionResolver 코드 파헤치기
예외를 특정 HTTP 상태 코드에 매핑하고 해당 코드와 오류 메시지로 응답을 생성합니다. `response.sendError`가 사용되므로, 예외가 올라가서 처리되는 것을 알 수 있음.
```java
protected ModelAndView doResolveException(  
        HttpServletRequest request, HttpServletResponse response, @Nullable Object handler, Exception ex) {  
  
    try {  
        // 생략...  
        else if (ex instanceof TypeMismatchException) {  
            return handleTypeMismatch( (TypeMismatchException) ex, request, response, handler);  
        }  
        // 생략...  
    } catch (Exception handlerEx) {  
        if (logger.isWarnEnabled()) {  
            logger.warn("Failure while trying to resolve exception [" + ex.getClass().getName() + "]", handlerEx);  
        }  
    }  
    return null;  
}

protected ModelAndView handleHttpRequestMethodNotSupported(HttpRequestMethodNotSupportedException ex, 
			HttpServletRequest request, HttpServletResponse response, @Nullable Object handler) throws IOException {  

    String[] supportedMethods = ex.getSupportedMethods();  
    
    if (supportedMethods != null) {  
        response.setHeader("Allow", StringUtils.arrayToDelimitedString(supportedMethods, ", "));  
    }  
  
    response.sendError(405, ex.getMessage());  
    return new ModelAndView();  
}
```

