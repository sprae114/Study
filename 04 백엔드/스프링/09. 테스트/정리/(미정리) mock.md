# Mock 개념
### 1. Mock
스프링에서 "mock"은 테스트 중 실제 빈(Bean)이나 외부 시스템(데이터베이스, API 등)을 사용하지 않고, 그 동작을 흉내 내는 가짜 객체를 의미합니다. 이는 단위 테스트(Unit Test)와 통합 테스트(Integration Test)에서 자주 사용되며, 다음 목적을 가집니다:
- **의존성 분리**: 테스트 대상 코드가 의존하는 객체를 모의 객체로 대체해 독립적으로 테스트.
- **속도 향상**: 실제 DB나 네트워크 호출 대신 mock을 사용해 테스트 속도를 빠르게.
- **예외 상황 시뮬레이션**: 실제로는 재현하기 어려운 에러나 특수 상황을 mock으로 쉽게 테스트.

스프링에서는 주로 **Mockito**라는 mocking 프레임워크와 스프링의 테스트 모듈(`spring-test`)을 결합해 이를 구현합니다.

---

### 2. **주요 도구**
스프링에서 mock을 다룰 때 자주 사용되는 도구는 다음과 같습니다:
- **Mockito**: Java에서 가장 널리 사용되는 mocking 라이브러리. `@Mock`, `@InjectMocks` 등의 애너테이션 제공.
- **Spring Test**: 스프링 컨텍스트를 로드하거나 특정 빈을 mock으로 대체할 수 있게 지원.
- **`@MockBean`**: 스프링 부트에서 제공하는 애너테이션으로, 스프링 애플리케이션 컨텍스트 내의 빈을 mock으로 대체.

---

### 3. **Mockito와 스프링 통합**
스프링에서 mock을 사용할 때 일반적으로 Mockito를 활용합니다. 이를 스프링 테스트에 통합하는 기본 설정은 다음과 같습니다:

#### **의존성 추가 (Maven 기준)**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```
- `spring-boot-starter-test`는 JUnit, Mockito, Spring Test 등을 포함합니다.

#### **기본 테스트 구조**
```java
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.boot.test.context.SpringBootTest;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class) // Mockito를 JUnit 5와 통합
public class MyServiceTest {

    @Mock
    private MyRepository myRepository; // 의존성 mock

    @InjectMocks
    private MyService myService; // 테스트 대상 객체에 mock 주입

    @Test
    public void testServiceMethod() {
        // Mock 동작 정의
        when(myRepository.findById(1L)).thenReturn("Mocked Data");

        // 서비스 호출
        String result = myService.getData(1L);

        // 결과 검증
        assertEquals("Mocked Data", result);
        verify(myRepository, times(1)).findById(1L); // 호출 검증
    }
}
```
- **`@Mock`**: `MyRepository`를 가짜 객체로 생성.
- **`@InjectMocks`**: `MyService`에 mock 객체를 주입.
- **`when(...).thenReturn(...)`**: 특정 메서드 호출 시 반환값 정의.
- **`verify`**: mock 객체가 예상대로 호출되었는지 확인.

---

### 4. **스프링 부트의 `@MockBean` 활용**
스프링 애플리케이션 컨텍스트를 사용하는 테스트에서 `@MockBean`을 활용하면, 컨텍스트 내의 실제 빈을 mock으로 대체할 수 있습니다.

#### **예제**
```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import static org.mockito.Mockito.*;

@SpringBootTest
public class MyServiceIntegrationTest {

    @Autowired
    private MyService myService;

    @MockBean
    private MyRepository myRepository; // 컨텍스트 내 빈을 mock으로 대체

    @Test
    public void testWithMockBean() {
        // Mock 동작 정의
        when(myRepository.findById(1L)).thenReturn("Mocked Data");

        // 서비스 호출
        String result = myService.getData(1L);

        // 검증
        assertEquals("Mocked Data", result);
        verify(myRepository).findById(1L);
    }
}
```
- **`@SpringBootTest`**: 스프링 애플리케이션 컨텍스트를 로드.
- **`@MockBean`**: `MyRepository` 빈을 mock으로 교체해 테스트.

#### **장점**
- 실제 DB 연결이나 외부 API 호출 없이 테스트 가능.
- 컨텍스트 내 다른 빈은 유지하면서 특정 빈만 mock으로 대체 가능.

---

### 5. **실제 사용 사례**
#### **상황**: 서비스 계층 테스트
- `OrderService`가 `OrderRepository`(DB 접근)와 `PaymentApi`(외부 API)에 의존한다고 가정.
- DB와 API 호출을 mock으로 대체해 `OrderService`만 테스트.

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentApi paymentApi;

    public OrderService(OrderRepository orderRepository, PaymentApi paymentApi) {
        this.orderRepository = orderRepository;
        this.paymentApi = paymentApi;
    }

    public String processOrder(Long orderId) {
        String order = orderRepository.findOrder(orderId);
        paymentApi.charge(orderId, 100.0);
        return "Processed: " + order;
    }
}

@SpringBootTest
public class OrderServiceTest {

    @Autowired
    private OrderService orderService;

    @MockBean
    private OrderRepository orderRepository;

    @MockBean
    private PaymentApi paymentApi;

    @Test
    public void testProcessOrder() {
        // Mock 동작 정의
        when(orderRepository.findOrder(1L)).thenReturn("Order #1");
        doNothing().when(paymentApi).charge(1L, 100.0);

        // 테스트 실행
        String result = orderService.processOrder(1L);

        // 검증
        assertEquals("Processed: Order #1", result);
        verify(orderRepository).findOrder(1L);
        verify(paymentApi).charge(1L, 100.0);
    }
}
```
- **`doNothing()`**: void 메서드의 동작을 정의할 때 사용.

---

### 6. **주의점**
- **Mock 남용 금지**: 모든 것을 mock으로 대체하면 실제 동작과 괴리가 생길 수 있으니, 단위 테스트와 통합 테스트를 적절히 조합.
- **컨텍스트 로드 비용**: `@SpringBootTest`와 `@MockBean`을 사용할 경우 전체 컨텍스트가 로드되므로, 단순 단위 테스트에는 Mockito만 사용하는 것이 더 빠를 수 있음.
- **의존성 주입 방식**: 생성자 주입을 선호하면 `@InjectMocks`와의 호환성이 좋아짐.

# mock에서 자주 사용하는 어노테이션, 함수

스프링에서 "mock"을 사용할 때 주로 **Mockito**와 스프링의 테스트 모듈(`spring-test`, `spring-boot-test`)을 활용한다고 앞서 설명드렸습니다. 이번에는 Mockito와 스프링 테스트에서 자주 사용하는 **어노테이션**과 **함수**를 중심으로 자세히 정리해서 알려드리겠습니다. 각 항목의 역할과 사용 예시를 포함해 실무에서 바로 적용할 수 있도록 하겠습니다.

---

### 1. **자주 사용하는 어노테이션**
Mockito와 스프링에서 mock 테스트를 할 때 자주 쓰이는 어노테이션은 다음과 같습니다.

#### **Mockito 관련 어노테이션**
1. **`@Mock`**
   - **설명**: 특정 객체를 mock(가짜 객체)로 생성.
   - **사용**: 의존성을 가진 객체를 mock으로 대체할 때 사용.
   - **예시**:
     ```java
     @Mock
     private MyRepository myRepository;
     ```
     - `myRepository`가 실제 구현체가 아닌 mock 객체로 생성됨.

2. **`@InjectMocks`**
   - **설명**: 테스트 대상 객체에 `@Mock`으로 생성된 mock 객체를 자동으로 주입.
   - **사용**: 주로 생성자 주입, setter 주입, 필드 주입 방식에서 동작.
   - **예시**:
     ```java
     @InjectMocks
     private MyService myService;
     ```
     - `myService`에 `@Mock`으로 생성된 의존성(`myRepository`)이 주입됨.

3. **`@Spy`**
   - **설명**: 실제 객체를 기반으로 부분적으로 mock 기능을 추가. 메서드 호출을 가로채지 않으면 실제 동작을 수행.
   - **사용**: 특정 메서드만 mock하고 나머지는 실제 동작을 유지하고 싶을 때.
   - **예시**:
     ```java
     @Spy
     private MyService myService = new MyServiceImpl();
     ```

4. **`@ExtendWith(MockitoExtension.class)`**
   - **설명**: JUnit 5에서 Mockito를 사용할 수 있게 해주는 확장. `@Mock`, `@InjectMocks` 등을 활성화.
   - **사용**: 클래스 레벨에 추가.
   - **예시**:
     ```java
     @ExtendWith(MockitoExtension.class)
     public class MyTest {
     ```

#### **스프링 관련 어노테이션**
5. **`@MockBean`**
   - **설명**: 스프링 애플리케이션 컨텍스트 내의 빈을 mock으로 대체.
   - **사용**: 통합 테스트에서 실제 빈을 mock으로 교체할 때.
   - **예시**:
     ```java
     @SpringBootTest
     public class MyIntegrationTest {
         @MockBean
         private MyRepository myRepository;
     ```
     - 컨텍스트 내 `MyRepository` 빈이 mock으로 대체됨.

6. **`@SpringBootTest`**
   - **설명**: 스프링 애플리케이션 컨텍스트를 로드해 테스트 환경을 구성.
   - **사용**: `@MockBean`과 함께 사용해 통합 테스트를 지원.
   - **예시**: 위 `@MockBean` 예시 참고.

7. **`@Autowired`**
   - **설명**: 스프링 컨텍스트에서 빈을 주입받음. `@MockBean`과 함께 사용 시 mock 객체도 주입 가능.
   - **예시**:
     ```java
     @Autowired
     private MyService myService; // @MockBean으로 대�된 의존성을 포함
     ```

---

### 2. **자주 사용하는 함수 (Mockito 중심)**
Mockito에서 mock 객체의 동작을 정의하거나 검증할 때 자주 사용하는 주요 메서드는 다음과 같습니다. 이들은 `static` 메서드로, `org.mockito.Mockito`에서 임포트합니다.

#### **Mock 객체 동작 정의**
1. **`when(...).thenReturn(...)`**
   - **설명**: mock 객체의 메서드가 호출될 때 반환할 값을 정의.
   - **사용**: 특정 입력에 대한 출력값을 설정.
   - **예시**:
     ```java
     when(myRepository.findById(1L)).thenReturn("Mocked Data");
     ```
     - `findById(1L)` 호출 시 `"Mocked Data"` 반환.

2. **`when(...).thenThrow(...)`**
   - **설명**: mock 객체의 메서드가 호출될 때 예외를 발생시키도록 정의.
   - **사용**: 예외 처리 로직을 테스트할 때.
   - **예시**:
     ```java
     when(myRepository.findById(1L)).thenThrow(new RuntimeException("Not found"));
     ```

3. **`doReturn(...).when(...)`**
   - **설명**: `when(...).thenReturn(...)`의 대체 방식으로, void 메서드 호출 전에 사용 가능.
   - **사용**: 메서드 호출 순서를 명확히 하거나 스파이 객체에서 사용.
   - **예시**:
     ```java
     doReturn("Mocked Data").when(myRepository).findById(1L);
     ```

4. **`doThrow(...).when(...)`**
   - **설명**: void 메서드에서 예외를 던지도록 설정.
   - **예시**:
     ```java
     doThrow(new RuntimeException()).when(myRepository).deleteById(1L);
     ```

5. **`doNothing().when(...)`**
   - **설명**: void 메서드가 호출될 때 아무 동작도 하지 않도록 설정.
   - **사용**: 기본적으로 아무것도 하지 않는 void 메서드를 명시적으로 설정.
   - **예시**:
     ```java
     doNothing().when(paymentApi).charge(1L, 100.0);
     ```

#### **Mock 호출 검증**
6. **`verify(...)`**
   - **설명**: mock 객체의 메서드가 호출되었는지 확인.
   - **사용**: 특정 메서드가 예상대로 호출되었는지 검증.
   - **예시**:
     ```java
     verify(myRepository).findById(1L);
     ```

7. **`verify(..., times(n))`**
   - **설명**: 메서드가 특정 횟수만큼 호출되었는지 확인.
   - **예시**:
     ```java
     verify(myRepository, times(2)).findById(1L); // 2번 호출 확인
     ```

8. **`verify(..., never())`**
   - **설명**: 메서드가 한 번도 호출되지 않았는지 확인.
   - **예시**:
     ```java
     verify(myRepository, never()).deleteById(1L);
     ```

9. **`verifyNoInteractions(...)`**
   - **설명**: mock 객체와의 상호작용이 전혀 없었는지 확인.
   - **예시**:
     ```java
     verifyNoInteractions(myRepository);
     ```

#### **기타 유용한 함수**
10. **`any()` / `anyInt()` / `anyString()`**
    - **설명**: 메서드 인자의 특정 값을 무시하고 어떤 값이든 매칭되도록 설정.
    - **사용**: 인자 값이 중요하지 않을 때.
    - **예시**:
      ```java
      when(myRepository.findByName(anyString())).thenReturn("Mocked");
      ```

11. **`eq(...)`**
    - **설명**: 특정 인자 값과 정확히 일치하도록 설정.
    - **예시**:
      ```java
      when(myRepository.findById(eq(1L))).thenReturn("Mocked");
      ```

---

### 3. **실제 예제**
위 어노테이션과 함수를 결합한 간단한 테스트 예제입니다.

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
public class MyServiceTest {

    @Mock
    private MyRepository myRepository;

    @InjectMocks
    private MyService myService;

    @Test
    public void testGetData() {
        // 동작 정의
        when(myRepository.findById(anyLong())).thenReturn("Mocked Data");
        doThrow(new RuntimeException("Error")).when(myRepository).deleteById(1L);

        // 테스트 실행
        String result = myService.getData(1L);

        // 검증
        assertEquals("Mocked Data", result);
        verify(myRepository, times(1)).findById(1L);
        verify(myRepository, never()).findById(2L);
    }
}
```

# 서비스 코드 작성하는법
