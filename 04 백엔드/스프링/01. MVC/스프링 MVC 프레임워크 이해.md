#MVC, #애노테이션

#인프런MVC_1편02

----
## 직접 만든 MVC와 스프링 MVC와 비교

#### 그림으로 비교하기 ✔
![[Pasted image 20221219213807.png]]

#### 용어 정리  
- handle란? **변환하기 전 URI 정보**  
- adapter란? handle를 알맞은 **타입으로 변환**시키기 위한 메소드  
- ModelAndView -> View 이름, View를 렌더링할 때 필요한 Model 객체나 String(**비즈니스 로직 처리한데이터**)  
- viewResolver -> **View이름을 URI로 만들기 위한 메소드**(렌더링하기 위함)  

#### 매커니즘 정리  
1) 핸들러 조회  
- 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.  
- url 뿐만 아니라 content-type 등 여러 데이터를 확인한다.  

2) 핸들러 어댑터 조회  
- 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.  

3) 핸들러 어댑터 실행  
- 핸들러 어댑터를 실행한다.  

4) ModelAndView 반환  
- 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.  

5) viewResolver 호출  
- viewResolver를 찾고 호출한다.  
- JSP는 InternalResourceViewResolver가 자동으로 등록되어 사용된다.  

6) View 반환  
- viewResolver는 view의 논리 이름을 물리 이름으로 바꾸고 렌더링 역할을 담당하는 View 객체를 반환한다.  
- JSP는 InternalResourceView를 반한하며 이 내부에 forward() 로직이 있다.  

7) view 렌더링  
- View를 통해 view를 렌더링한다.  


#### (코드) 직접만든 MVC 프레임워크
```java
@WebServlet(name = "frontControllerServletV5", urlPatterns = "/front-controller/v5/*")  
public class FrontControllerServletV5 extends HttpServlet {  
  
    private final Map<String, Object> handlerMappingMap = new HashMap<>();  
    private final List<MyHandlerAdapter> handlerAdapters = new ArrayList<>();  
  
    public FrontControllerServletV5() {  
        initHandlerMappingMap();  
        initHandlerAdapters();  
    }  

    //0.핸들러 등록하기  
    private void initHandlerMappingMap() {  
	    //V3 추가
        handlerMappingMap.put("/front-controller/v5/v3/members/new-form", new MemberFormControllerV3());  
        handlerMappingMap.put("/front-controller/v5/v3/members/save", new MemberSaveControllerV3());  
        handlerMappingMap.put("/front-controller/v5/v3/members", new MemberListControllerV3());  
  
        //V4 추가  
        handlerMappingMap.put("/front-controller/v5/v4/members/new-form", new MemberFormControllerV4());  
        handlerMappingMap.put("/front-controller/v5/v4/members/save", new MemberSaveControllerV4());  
        handlerMappingMap.put("/front-controller/v5/v4/members", new MemberListControllerV4());  
    }  
  
    //0.어댑터 등록하기  
    private void initHandlerAdapters() {  
        handlerAdapters.add(new ControllerV3HandlerAdapter());  
        handlerAdapters.add(new ControllerV4HandlerAdapter());  
    }  
  
    @Override  
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
        //1.URL에 해당하는 핸들러 얻기. 없으면 404 오류페이지  
        Object handler = getHandler(request);  
        if (handler == null) {  
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);  
            return;        }  
  
        //3.핸들러에 맞는 어댑터 찾기  
        MyHandlerAdapter adapter = getHandlerAdapter(handler);  
  
        //4.핸들러 호출해서 비즈니스로직 수행하기  
        ModelView mv = adapter.handle(request, response, handler);  
  
        //5.ModelView에서 값 반환하기  
        String viewName = mv.getViewName();  
        
        //6~7.viewResolver 호출해서 해당하는 뷰 반환하기   
		MyView view = viewResolver(viewName);  
  
        //8.view 랜더링하기  
        view.render(mv.getModel(), request, response);  
  
    }  


    private Object getHandler(HttpServletRequest request) {  
        String requestURI = request.getRequestURI();  
        return handlerMappingMap.get(requestURI);  
    }  


    private MyHandlerAdapter getHandlerAdapter(Object handler) {  
        //MemberFormControllerV4  
        for (MyHandlerAdapter adapter : handlerAdapters) {  
            if (adapter.supports(handler)) {  
                return adapter;  
            }  
        }  
        throw new IllegalArgumentException("handler adapter를 찾을 수 없습니다. handler=" + handler);  
    }  


    private MyView viewResolver(String viewName) {  
        return new MyView("/WEB-INF/views/" + viewName + ".jsp");  
    }  
}
```


#### (코드) 스프링에서 제공하는 DispatcherServlet
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {  
  
    HttpServletRequest processedRequest = request;  
    HandlerExecutionChain mappedHandler = null;  
    ModelAndView mv = null;  
  
    // 1. 핸들러 조회  
    mappedHandler = getHandler(processedRequest);  
    if (mappedHandler == null) {  
        noHandlerFound(processedRequest, response);  
        return;    
    }  
  
    //2.핸들러 어댑터 조회-핸들러를 처리할 수 있는 어댑터  
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());  
  
    // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환   
	mv = ha.handle(processedRequest, response, mappedHandler.getHandler());  
  
    processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);  
}  


private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {  
    // 뷰 렌더링 호출  (렌더링 한다는 사실이 중요!!!)
    render(mv, request, response); 
}  
  
protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response)  
throws Exception {  
    View view;  
    String viewName = mv.getViewName();  
  
    //6. 뷰 리졸버를 통해서 뷰 찾기, 7.View 반환  
    view = resolveViewName(viewName, mv.getModelInternal(), locale, request);  
  
    // 8. 뷰 렌더링  
    view.render(mv.getModelInternal(), request, response);  
}
```


## 스프링 MVC 적용하기
#### (코드) ModelAndView를 이용한 회원 등록 폼만들기
```java
@Controller  
@RequestMapping("/springmvc/v2/members")  
public class SpringMemberControllerV2 {  
  
    private MemberRepository memberRepository = MemberRepository.getInstance();  
  
    @RequestMapping("/new-form")  
    public ModelAndView newForm() {  
        return new ModelAndView("new-form");  
    }  
  
    @RequestMapping("/save")  
    public ModelAndView save(HttpServletRequest request, HttpServletResponse response) {  
        String username = request.getParameter("username");  
        int age = Integer.parseInt(request.getParameter("age"));  
  
        Member member = new Member(username, age);  
        memberRepository.save(member);  
  
        ModelAndView mv = new ModelAndView("save-result");  
        mv.addObject("member", member);  
        return mv;  
    }  
  
    @RequestMapping  
    public ModelAndView members() {  
  
        List<Member> members = memberRepository.findAll();  
  
        ModelAndView mv = new ModelAndView("members");  
        mv.addObject("members", members);  
        return mv;  
    }  
}
```

#### (코드)실용적 방식의 회원 등록 폼만들기

```java
@Controller  
@RequestMapping("/springmvc/v3/members")  
public class SpringMemberControllerV3 {  
  
    private MemberRepository memberRepository = MemberRepository.getInstance();  
  
    @GetMapping("/new-form")  
    public String newForm() {  
        return "new-form";  
    }  
  
    @PostMapping("/save")  
    public String save(  
            @RequestParam("username") String username,  
            @RequestParam("age") int age,  
            Model model) {  
  
        Member member = new Member(username, age);  
        memberRepository.save(member);  
  
        model.addAttribute("member", member);  
        return "save-result";  
    }  
  
    @GetMapping  
    public String members(Model model) {  
  
        List<Member> members = memberRepository.findAll();  
  
        model.addAttribute("members", members);  
        return "members";  
    }  
}
```
