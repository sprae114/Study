# Validation

# 1. 매커니즘

## 1) 그림으로 보는 검증 처리 매커니즘

- post 요청을 정상적으로 처리한 경우
![[Untitled5.png]]

- post 요청을 실패한 경우
![[img1.daumcdn.png]]

---

## 2)초창기 검증을 보며, 이해하기

- 검증로직(Controller)
    
    ```java
    @PostMapping("/add")
    public String addItemV1(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {
    
        // 검증 로직
        if(!StringUtils.hasText(item.getItemName())){
            bindingResult.addError(new FieldError("item", "itemName", "상품 이름은 필수 입니다."));
        }
        if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
            bindingResult.addError(new FieldError("item", "price", "가격은 1,000 ~ 1,000,000 까지 허용합니다."));
        }
        if(item.getQuantity() == null || item.getQuantity() >= 9999){
            bindingResult.addError(new FieldError("item", "quantity", "수량은 최대 9,999 까지 허용합니다."));
        }
    
        // 복합 룰 검증
        if(item.getPrice() != null && item.getQuantity() != null){
            int resultPrice = item.getPrice() * item.getQuantity();
            if(resultPrice < 10000){
                bindingResult.addError(new ObjectError("item", "가격 * 수량의 합은 10,000원 이상이어야 합니다."));
            }
        }
    <form action="item.html" th:action th:object="${item}" method="post">
        <div th:if="${#fields.hasGlobalErrors()}">
        <p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">글로벌 오류 메시지</p>
        </div>
        <div>
            <label for="itemName" th:text="#{label.item.itemName}">상품명</label>
            <input type="text" id="itemName" th:field="*{itemName}"
                   th:errorclass="field-error" class="form-control" placeholder="이름을 입력하세요">
            <div class="field-error" th:errors="*{itemName}">
                상품명 오류
            </div>
        // 검증에 실패한 경우
        if(bindingResult.hasErrors()){
            log.info("errors= {}", bindingResult);
            return "validation/v2/addForm";
        }
    
        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }
    ```
    

- 정리
    
    1) 검증 오류 결과 보관하기 → bindingResult
    
    2) 검증 로직(필드 검증, 복합 검증) 
    
    3) 오류가 있는경우 예외처리 하기
    
    남은 문제점은 타입 검증, null 처리하기
    

- Html
    
    ```html
    <!-- 생략 -->
    
    <div class="py-5 text-center">
            <h2 th:text="#{page.addItem}">상품 등록</h2>
        </div>
    
        <form action="item.html" th:action th:object="${item}" method="post">
            <div th:if="${errors?.containsKey('globalError')}">
            <p class="field-error" th:text="${errors['globalError']}">전체 오류 메시지</p>
            </div>
            <div>
                <label for="itemName" th:text="#{label.item.itemName}">상품명</label>
                <input type="text" id="itemName" th:field="*{itemName}"
                       th:class="${errors?.containsKey('itemName')} ? 'form-control field-error' : 'form-control'"
                       class="form-control" placeholder="이름을 입력하세요">
                <div class="field-error" th:if="${errors?.containsKey('itemName')}" th:text="${errors['itemName']}">
                    상품명 오류
                </div>
            </div>
    
    <!-- 생략 -->
    ```
    

- 정리
    - thymleaf 정리
        - **#fields** : 타임리프가 제공하는 기본 객체이다. #fields로 BindingResult가 제공하는 검증 오류에 접근할 수 있다.
        - **th:errors** : 해당 필드에 오류가 있는 경우에 태그를 출력한다. th:if 의 편의 버전이다.
        - **th:errorclass** : th:field 에서 지정한 필드 이름으로 BindfingResult에 오류가 있으면 class 정보를 추가한다.
        
    - 글로벌 오류 처리
        
        ```html
        <div th:if="${#fields.hasGlobalErrors()}">
              <p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">전체 오류 메시지</p> 
        </div>
        ```
        
    - 필드 오류 처리
        
        ```html
        <input type="text" id="itemName" th:field="*{itemName}" 
        th:errorclass="field-error" class="form-control" placeholder="이름을입력하세요">
        <div class="field-error" th:errors="*{itemName}"> 상품명 오류 </div>
        ```
        
    

## 2)글로 보는 코드 구성

- 글로 보는 코드 구성
    1. 검증기 만들어서 Validator 컨테이너에 등록하기
    
     2. controller에 @Validation을 이용해서 적용하기
    

---

실제 코드 순서

1) 의존성 추가

2) BeanValidation을 이용해 타입 검증 및 필드 검증 

3) html에 검증 나타낼 수 있게 추가

4) 해당 매소드에 `BindingResult` 를 추가 ⇒ 검증 오류 저장하는 곳

5) 검증 걸리면 예외 걸리게 코드 추가

6) 오브젝트 검증(비즈니스 검증) Controller에 추가

7) `BindingResult` 에 직접 오브젝트 검증 추가 후, 예외 걸리는 코드 작성

## 3) 실제 코드 예시

Validator 코드 추가하기

```java
@Component
@RequiredArgsConstructor
public class PostFormValidator implements Validator { //Validator 상속하기

    private final PostRepository postRepository; //

    @Override
    public boolean supports(Class<?> clazz) {
        return clazz.isAssignableFrom(PostForm.class);
    }

    @Override
    public void validate(Object target, Errors errors) {

        PostForm postForm = (PostForm)target;
        if (postRepository.existsByTitle(postForm.getTitle())) {
            errors.rejectValue("title", "invalid.title", new Object[]{postForm.getTitle()}, "중복된 꿈 명이 존재합니다.");
        }

        if (postRepository.existsByContents(postForm.getContents())) {
            errors.rejectValue("contents", "invalid.contents", new Object[]{postForm.getContents()}, "중복된 꿈 내용이 존재합니다.");
        }
    }
```

- 코드 구성
    1. @Component하여 검증기를 컨테이너에 등록하기
    2. Validator 상속하기
    3. 

Controller 매핑

```java
@PostMapping("URL")
public String validationPage(@Valid @ModelAttribute PostForm postForm, Errors errors)
	if (errors.hasErrors()){return "post/save-dream"}
	
		Post post = new Post(postForm);
	  postRepository.save(post);
	
	  return "redirect:/";
}
```

## 2. 검증 종류

- **타입 검증**
    
    원하는 타입만 들어가도록 하는 검증
    
    예) 가격, 수량에 문자가 들어가면 검증 오류 처리
    
- **필드 검증**
    
    숫자일 때, 필수 값 및 원하는 범위 설정
    
    예) 상품명: 필수, 공백X, 
    
    예) 가격: 1000원 이상
    
    예) 수량: 최대 9999
    
- **오브젝트 검증**
    
    2개 이상의 필드가 로직을 이루어질 때 범위 설정
    
    예) 가격 * 수량의 합은 10,000원 이상
    
- **API 검증**
    
    postman같은 Http요청으로 데이터 조작 가능함으로 방어하기 위함
    

## 3. 검증 사용 방법

```java

```

```java

```

```java

```

```java

```

```java

```

0) validation 의존성 추가

- gradle 추가

1) Bean Validation

```java
public class Item {
      private Long id;

      @NotBlank
      private String itemName;

      @NotNull
      @Range(min = 1000, max = 1000000)
      private Integer price;
  
      @NotNull
      @Max(9999)
      private Integer quantity;

      //...
}
```