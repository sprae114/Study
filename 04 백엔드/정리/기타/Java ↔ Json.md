
#jackson, #JSON , #변환

---
# jackson
## 개념
![[Pasted image 20250312004636.png]]
**JSON과 Java 객체 간의 매핑을 처리**합니다. 주요 기능은 다음과 같습니다:
- **직렬화(Serialization)**: Java 객체를 JSON 문자열로 변환.
- **역직렬화(Deserialization)**: JSON 문자열을 Java 객체로 변환.
- **커스터마이징**: 날짜 형식, null 값 처리, 속성 이름 매핑 등을 사용자 정의 가능.


## 설정
`spring-boot-starter-web`에 기본 포함되므로, **별도 추가 없이 사용 가능**. 
![[Pasted image 20250312115739.png]]


## 기본 사용 (`ObjectMapper`) ✔
`ObjectMapper`는 `jackson-databind`의 핵심 클래스입니다.
### 직렬화(jave -> json)
```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Main {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();

        // Java 객체
        Person person = new Person("홍길동", 30);

        // JSON으로 변환
        String json = mapper.writeValueAsString(person);
        System.out.println(json); // {"name":"홍길동","age":30}
    }
}

@Getter
@Setter
@NoArgsConstructor  
@AllArgsConstructor
class Person {
    private String name;
    private int age;
}
```

### 역직렬화(json -> java)
```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Main {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();

        // JSON 문자열
        String json = "{\"name\":\"이순신\",\"age\":45}";

        // Java 객체로 변환
        Person person = mapper.readValue(json, Person.class);
        System.out.println(person.getName() + ", " + person.getAge()); // 이순신, 45
    }
}
```


### @RestController
Spring Boot에서는 `jackson-databind`가 기본적으로 포함되어 **REST 컨트롤러에서 자동으로 동작**합니다.
```java
import org.springframework.web.bind.annotation.*;

@RestController
public class PersonController {
    @GetMapping("/person")
    public Person getPerson() {
        return new Person(1L, "홍길동", 30); //@RestController에서는 object -> json 자동변환
    }

    @PostMapping("/person")
    public Person createPerson(@RequestBody Person person) {
		// @RequestBody에서 받은 Json객체 -> Object로 
        return person;
    }
}
```


### `@JsonProperty`, `@JsonIgnore`
`jackson-databind`는 다양한 어노테이션으로 직렬화/역직렬화를 세부 조정할 수 있습니다.
```java
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.databind.ObjectMapper;

public class Main {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();

        Person person = new Person("홍길동", 30, "secret");
        String json = mapper.writeValueAsString(person);
        System.out.println(json); // {"fullName":"홍길동","age":30}
    }
}

@Getter
@Setter
@NoArgsConstructor  
@AllArgsConstructor
class Person {
    @JsonProperty("fullName") // JSON 키를 "fullName"으로 변경
    private String name;

    private int age;

    @JsonIgnore // JSON에서 제외
    private String password;
}
```

## `ObjectMapper` 설정 ✔
### 기본 커스터마이징 
`ObjectMapper`를 설정하여 기본 동작을 변경할 수 있습니다.

▶ `ObjectMapper` 설정
```java
import com.fasterxml.jackson.databind.ObjectMapper; 
import com.fasterxml.jackson.databind.SerializationFeature; 
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule; 
import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateTimeSerializer; 
import com.fasterxml.jackson.databind.DeserializationFeature;
import java.time.format.DateTimeFormatter;
import org.springframework.context.annotation.Bean; 
import org.springframework.context.annotation.Configuration;

@Configuration
public class JacksonConfig {
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        
        // Null 값 제외
        mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
        
        // 예쁜 출력
        mapper.enable(SerializationFeature.INDENT_OUTPUT);
        
        // Java 8 날짜/시간 처리
        JavaTimeModule module = new JavaTimeModule();
        module.addSerializer(LocalDateTime.class, 
            new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        mapper.registerModule(module);
        mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        
        // 알 수 없는 속성 무시
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        return mapper;
    }
}
```

▶ 객체
```java
@Getter
@Setter
@NoArgsConstructor  
@AllArgsConstructor
@ToString
class Person {
    private String name; 
    private Integer age; // null 값 테스트용 
    private LocalDateTime birthDate; 
    private String extraField; // 알 수 없는 속성 테스트용 (JSON에만 존재)
}
```

▶ 컨트롤러
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.time.LocalDateTime;

@RestController
public class PersonController {

    @GetMapping("/person")
    public Person getPerson() {
        return new Person("홍길동", null, LocalDateTime.now());
    }

    @PostMapping("/person")
    public Person createPerson(@RequestBody Person person) {
        return person;
    }
}
```

▶ 테스트
- GET 요청 : `http://localhost:8080/person`
```json
{
  "name" : "홍길동",
  "birthDate" : "2025-03-11 14:30:00"
}
```

- POST 요청 : `http://localhost:8080/person` 에 
`{"name":"이순신","age":45,"birthDate":"2025-03-11 15:00:00","unknown":"test"}`

```json
{
  "name" : "이순신",
  "age" : 45,
  "birthDate" : "2025-03-11 15:00:00"
}
```


### redis
추후에..

### kafka
추후에..

## 주의사항 ✔
- **기본 생성자와 Setter** : `@RequestBody`로 역직렬화할 때 기본 생성자와 setter가 없으면 `JsonMappingException` 발생. 그 이유는 빈 객체 생성 후, setter 메서드를 통해 값을 주입하기 때문임 .
- **Java 8 날짜/시간 API 지원** : `LocalDateTime`, `LocalDate` 등 Java 8 날짜/시간 객체를 사용하려면 `jackson-datatype-jsr310` 모듈이 필요함. 


# Java 8 날짜/시간

## jackson-datatype ✔
### 설정
`LocalDateTime` 등은 기본 지원되지 않으며, `jackson-datatype-jsr310` 추가 필요.
```gradle
implementation 'com.fasterxml.jackson.datatype:jackson-datatype-jsr310'
```

### 사용방법
`ObjectMapper`에 **`JavaTimeModule`을 등록**하여 `LocalDateTime`과 같은 객체를 처리할 수 있도록 설정합니다.

▶ config 등록
```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JacksonConfig {

    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        // JavaTimeModule 등록
        mapper.registerModule(new JavaTimeModule());
        // 타임스탬프 대신 ISO-8601 형식으로 직렬화
        mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        return mapper;
    }
}
```
- `JavaTimeModule` : Java 8 날짜/시간 API(`LocalDateTime`, `LocalDate`, `ZonedDateTime` 등)를 지원.
- `WRITE_DATES_AS_TIMESTAMPS` : 기본적으로 `Jackson`은 날짜를 타임스탬프로 변환하는데, 이 설정을 비활성화하면 ISO-8601 형식(예: `2025-03-11T14:30:00`)으로 출력됩니다.


▶ 컨트롤러
```java
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;

@RestController
public class EventController {

    @GetMapping("/event")
    public Event getEvent() {
        return new Event(1L, LocalDateTime.of(2025, 3, 11, 14, 30, 0));
    }

    @PostMapping("/event")
    public Event createEvent(@RequestBody Event event) {
        return event; // 받은 객체 그대로 반환
    }
}
```

- GET : `http://localhost:8080/event`
```json
{
  "id" : "l",
  "startTime": "2025-03-11 14:30:00"
}
```

- POST : `http://localhost:8080/event`에 `{"startTime": "2025-03-12 15:45:00"}` 
```json
{
  "id" : "2"
  "startTime": "2025-03-12 15:45:00"
}
```


## @JsonFormat  ✔
### 개념
Jackson 라이브러리에서 제공하는 어노테이션으로, 주로 **날짜/시간 객체**(예: LocalDateTime, Date)와 **열거형(enum)** 의 JSON 표현 방식을 커스터마이징하는 데 사용됩니다.


### 설정
Java 8 날짜/시간 객체를 사용하려면 `jackson-datatype-jsr310` 모듈이 필요합니다.
1) 의존성 등록
```gradle
implementation 'com.fasterxml.jackson.datatype:jackson-datatype-jsr310'
```

2) 설정 등록
```java
@Bean
public ObjectMapper objectMapper() {
    ObjectMapper mapper = new ObjectMapper();
    mapper.registerModule(new JavaTimeModule()); // Java 8 날짜/시간 지원
    mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);   // 기본 타임스탬프 비활성화
    return mapper;
}
```


### 언제 사용할까?
1) 날짜/시간 객체의 포맷 제어
기본적으로 LocalDateTime은 ISO-8601 형식(예: "2025-03-11T14:30:00")으로 직렬화되지만, 클라이언트가 다른 형식(예: "2025-03-11 14:30:00")을 요구할 때 사용.

2) 열거형(enum)의 표현 방식 변경
enum을 문자열(예: "ACTIVE") 대신 숫자(예: 0)로 출력하거나 반대로 설정.


### 사용방법
#### 날짜/시간
예를 들어, `startTime`만 지정된 포맷으로 직렬화되며, 같은 엔티티의 다른 `LocalDateTime` 필드(예: `endTime`)에는 영향을 주지 않습니다.

▶ 객체
```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
class Event { 
	private Long id;
	
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	private LocalDateTime startTime;
}
```

▶ 컨트롤러
```java
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;

@RestController
public class EventController {

    @GetMapping("/event")
    public Event getEvent() {
        return new Event(1L, LocalDateTime.of(2025, 3, 11, 14, 30, 0));
    }

    @PostMapping("/event")
    public Event createEvent(@RequestBody Event event) {
        return event; // 받은 객체 그대로 반환
    }
}
```

- GET : `http://localhost:8080/event`
```json
{
  "id" : "l",
  "startTime": "2025-03-11 14:30:00"
}
```

- POST : `http://localhost:8080/event`에 `{"startTime": "2025-03-12 15:45:00"}` 
```json
{
  "id" : "2"
  "startTime": "2025-03-12 15:45:00"
}
```


#### Enum
Enum은 Java에서 열거형 타입을 나타내며, JSON으로 직렬화(Serialization)하거나 역직렬화(Deserialization)할 때 기본적으로 이름(문자열)으로 변환됩니다. 
`@JsonFormat`을 사용하면 이를 숫자(인덱스)로 바꾸거나 다른 방식으로 커스터마이징할 수 있습니다.

▶ 객체
```java
import com.fasterxml.jackson.annotation.JsonFormat;

@Getter
@Setter
@NoArgsConstructor  
@AllArgsConstructor
public class User {
    private Long id;

    @JsonFormat(shape = JsonFormat.Shape.NUMBER)
    private Status statusNumber;

	@JsonFormat(shape = JsonFormat.Shape.STRING) 
	private Status statusString;

	@JsonFormat(shape = JsonFormat.Shape.OBJECT) 
	private Gender gender;
}

enum Status {
    ACTIVE,  // ordinal = 0
    INACTIVE // ordinal = 1
}

@AllArgsConstructor
@Getter
enum Gender {
	MALE("남자", 0),
	FEMALE("여자", 1)

	private final String description; 
	private final int code;
}
```

▶ 컨트롤러
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
    @GetMapping("/user")
    public User getUser() {
        return new User(1L, Status.ACTIVE, Status.ACTIVE, Gender.FEMALE);
    }

	@PostMapping("/user") 
	public User createUser(@RequestBody User user) { 
		return user; // 받은 객체 그대로 반환 
	}
}
```

- GET : `http://localhost:8080/user`
```json
{
  "id": 1,
  "statusNumber": 0
  "statusString" : "ACTIVE",
  "gender" : {
	  "description": "여자", 
	  "code": 1
  }
}
```

- POST : `http://localhost:8080/user`에 `{"id": 2, "statusNumber": 1, "statusString": "INACTIVE", "gender": {"description": "남자", "code": 0}}`

**문제**: `@JsonFormat(shape = OBJECT)`는 직렬화만 지원하며, 역직렬화는 기본적으로 동작하지 않음 → `JsonMappingException` 발생.

**해결방안** : 직렬화하려면 `JsonDeserializer`를 구현해야 합니다.

```java
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import com.fasterxml.jackson.databind.JsonNode;

import java.io.IOException;

public class GenderDeserializer extends JsonDeserializer<Gender> {
    
    @Override
    public Gender deserialize(JsonParser p, DeserializationContext ctxt) throws IOException {
        JsonNode node = p.getCodec().readTree(p);
        String description = node.get("description").asText();
        int code = node.get("code").asInt();

        for (Gender gender : Gender.values()) {
            if (gender.getDescription().equals(description) && gender.getCode() == code) {
                return gender;
            }
        }
        throw new IllegalArgumentException("Unknown Gender: " + description + ", " + code);
    }
}
```

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.module.SimpleModule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JacksonConfig {
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        SimpleModule module = new SimpleModule();
        module.addDeserializer(Gender.class, new GenderDeserializer());
        mapper.registerModule(module);
        return mapper;
    }
}
```


## 주의사항
- **필드 단위 적용**: `@JsonFormat`은 개별 필드에만 영향을 주며, 전역 설정(`ObjectMapper`)보다 우선순위가 높음.
- **타 시스템**: Redis, Kafka 등에서는 별도 직렬화 설정에 따라 `@JsonFormat`이 무시될 수 있음. 그래서 RedisConfig, KafKaConfig를 생성후 커스텀마이징해야함.