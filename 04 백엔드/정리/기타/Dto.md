#dto

---

## 예시
### 기본 형태
- record를 사용하여 dto를 쉽게 만듭니다.
```java
@Builder  
public record ProductDto(  
        String name,  
        int price
) {  
```


### Dto로 만들기
```java
public ProductDto from(String name, int price) {  
    return Product.builder()  
            .name(name)  
            .price(price)  
            .build();  
}
```


### 유효성 검사
- Bean Validation Annotation을 통해서 필드값의 유효성 검사를 합니다.
```java
@Builder  
public record ProductDto(  
        @NotEmpty  
        String name,  
        
        @Min(value = 0, message = "Age must be at least 0")  
        @Max(value = 120, message = "Age must be less than or equal to 120")  
        int price
) {  
```


### Json로 변환
- `toJson()`: DTO 객체를 JSON 문자열로 변환합니다. ObjectMapper를 사용하여 직렬화하고, 예외가 발생하면 스택 트레이스를 출력하고 null을 반환합니다.
```java
public String toJson() {  
    ObjectMapper objectMapper = new ObjectMapper();  
    try {  
        return objectMapper.writeValueAsString(this);  
    } catch (JsonProcessingException e) {  
        e.printStackTrace();  
        return null;  
    }  
}
```


### Entity로 변환
- `toEntity()`: ProductDto 객체를 Product 엔티티로 변환합니다. 
```java
public Product toEntity() {  
    return Product.builder()  
            .name(name)  
            .price(price)  
            .build();  
}
```