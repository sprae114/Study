
#매핑, #Post

#인프런MVC_1편03(page. )

----

# Post 매핑

## 1. 매커니즘
#### Post 요청 그림으로 파악하기
![[Untitled 3.png]]


#### 글로 코딩하기

- 글로 코딩하기
    1. Post요청하기
    2. Controller에서 Post 요청 받기
    3. Model과 함께 view로 보내기
    4. view는 html에서 데이터를 Model로 매핑시켜 응답하기


## 2. Request 요청 방법
#### 1) URL로 요청하기
💥인터넷에 URL로 요청하면 GET으로 처리됌. 그래서 postman을 써서 요청해야함.
- URL로 요청하기
![[Pasted image 20230112150206.png]]

#### 2) Body로 요청하기
- 바디에 직접 입력
  ![[Untitled 2.png]]

---

#### 3) Html로 요청하기
- input버튼으로 요청하는 방법
출처 : [코딩의 시작, TCP School](http://www.tcpschool.com/html-tag-attrs/form-action), [HTML DOM Form Object (w3schools.com)](https://www.w3schools.com/jsref/dom_obj_form.asp)
```html
<!-- 생략 -->
<form action="/request-param-v1" method="post">
		username: <input type="text" name="username" />
		age: <input type="text" name="age" />
		<button type="submit">전송</button>
</form>
<!-- 생략 -->
```
    
- 어떻게 전송되는 거지?
출처 : [th:action, th:onclick 차이 - 인프런 | 질문 & 답변 (inflearn.com)](https://www.inflearn.com/questions/522237/th-action-th-onclick-%EC%B0%A8%EC%9D%B4)
	onclick ->  클릭 시 발생할 메서드(이벤트)를 지정
    action → `<form>`태그에서 사용하는 속성으로 서식 데이터(form data)를 서버로 보낼 때 해당 데이터가 도착할 URL을 지정
    method → 동작 메소드 알려줌
    submit → 버튼을 통해 post 방식으로 th:action 요청함.

---

#### 4) Json로 요청하기

- Postman으로 Json 요청
    ![[Untitled 1.png]]


## 3. Controller에서 Post

#### 1) Post - @RequestParam
String, int, Long을 요청 파라미터로 받을 때 사용함.

- 기본 설정
```java
@ResponseBody
@PostMapping("/request-param-v3")
public String requestParamV3(
		@RequestParam String username,
		@RequestParam int age) {

	return "ok";
}
```



- 기본값 설정하기
```java
@ResponseBody
@PostMapping("/request-param-default")
public String requestParamDefault(
		@RequestParam(required = true, defaultValue = "guest") String username,
		@RequestParam(required = false, defaultValue = "-1") int age) {

	return "ok";
}
```
    
- required,는 필수 값, defaultValue는 기본 값
	required는 파라미터가 true면 요청 파라미터에 꼭 있어하는 것, false면 요청 파라미터에 없어도 되는 것
	defaultValue는 값을 입력하지 않을 때, 기본 값 지정함.

---
#### 2) Post - @PathVariable
URI 경로에 파라미터의 따라 변경하고 싶을 때

- **단일PathVariable** 사용
요청 매핑 : `http://localhost:8080/mapping/kang`

Controller
```java
@ResponseBody
@PostMapping("/mapping/{data}")
public String mappingPath(@PathVariable String data) {
	log.info("mappingPath userId={}", data);
	return "ok" + data;
}
```
    

- **다중 PathVariable** 사용
요청 매핑 :  `http://localhost:8080/mapping/kang/2`

Controller
```java
@ResponseBody
@PostMapping("/mapping/{data}/{num}")
public String mappingPath(@PathVariable String data, @PathVariable int num) {
	log.info("mappingPath userId={} {}", data, num);
	return "ok";
}
```
    

---

#### 3) Post - @RequestBody
출처 : https://mangkyu.tistory.com/72
- Json(application/json) 형태의 HTTP Body 데이터를 **MessageConverter를 통해 Java 객체로 변환**시킴
- 기본 생성자로 객체를 만들고, Getter나 Setter 등의 메소드로 필드를 찾아 Reflection으로 값을 설정함

- Controller
```java
@ResponseBody  
@PostMapping("/request-body-json-v5")  
public String requestBodyJsonV5(@RequestBody User user) {  
    log.info("username={}, age={}", user.getUsername(), user.getAge());  
    return user.toString();  
}
```


## 4. Model
Model을 넘겨준다는 의미는 DB에서 데이터를 받아 View로 랜더링함.

#### 1) Model
```java
@PostMapping("/add")
public String addItemV1(@RequestParam String itemName, @RequestParam int price, 
						@RequestParam Integer quantity, Model model) {

    Item item = new Item();
    item.setItemName(itemName);
    item.setPrice(price);
    item.setQuantity(quantity);

    itemRepository.save(item);
    model.addAttribute("item", item);

    return "basic/item";
}
```

---

#### 2) @ModelAttribute
1)과 같은 의미지만 입력에서 객체를 받을수 있게함.
```java
@PostMapping("/add")
public String addItemV2(@ModelAttribute("item") Item item) {
    itemRepository.save(item); 
    return "basic/item";
}
```

- @ModelAttribute의 역할
    1. 파라미터를 요청 받기   ex) `@RequestParam String itemName`
    2. 객체 생성하기   ex) `Item item = new Item();`
    3. 객체에 파라미터 넣기 ex) `item.setItemName(itemName);`
    4. model에 넣어서 view에 넘겨주기 ex) `model.addAttribute("item", item);`

---

#### 3) HttpEntity `<String>`
- **HTTP 메시지 바디**를 통해서 데이터가 넘어올때 사용함.
- **HttpEntity 를 사용하여 header, body의 정보를 편리하게 조회**
- HttpEntity , @RequestBody 를 사용하면 HTTP 메시지 컨버터가 HTTP 메시지 바디의 내용을 우리가 원하는 문자나 객체 등으로 변환해줌.
- HttpEntity는 **응답에도 사용 가능 메시지 바디 정보 직접 반환.  view 조회X**

```java
@ResponseBody
@PostMapping("/request-body-string-v3")
public HttpEntity<String> requestBodyStringV3(HttpEntity<String> httpEntity) throws IOException {
    HttpHeaders headers = httpEntity.getHeaders();
    log.info("messageBody={}", headers);

    return new HttpEntity<>("ok");
}
```


#### 4) @RequestBody
**HTTP 메시지 바디**를 통해서 데이터가 넘어올 때는 기존의 방식을 사용할 수가 없음.
```java
@ResponseBody
@PostMapping("/request-body-string-v4")
public String requestBodyStringV4(@RequestBody String messageBody) {
    log.info("messageBody={}", messageBody);
    return "OK";
}
```

- @RequestBody
    HTTP 메시지 바디 정보를 직접 편리하게 조회함.

---

## 5. View

#### 1) redirect 반환
resources에 있는 html의 이름이 매칭하여 view를 찾고 랜더링함. Post 방식은 멱등이 되지 않기 때문에 새로고침하면 결과에 영향을 줌. 따라서 redirect로 반환해야함.

Controller
```java
@PostMapping("/new-form")
public String newForm() {return "new-form";}
```

---

#### 2) json 반환
view를 찾는 것이 아니라, Http 메시지 바디에 바로 입력함.

- json으로 응답하기
```java
@RequestBody
@PostMapping("/hello-go")
public String helloBasic() {return "ok";}
```
- @RestController , @ResponseBody 의 공통된 역할은?
	- 반환 값으로 뷰를 찾는것이 아니라, HTTP 메시지 바디에 바로 입력하여 보낸다.


- ResponseEntity<> 를 이용한 응답
    HttpEntity는 HTTP 메시지의 헤더, 바디 정보를 가지고 있다
```java
@PostMapping("/response-body-string-v2")
public ResponseEntity<String> responseBodyV2() {
	return new ResponseEntity<>("ok", HttpStatus.OK);
}
```
