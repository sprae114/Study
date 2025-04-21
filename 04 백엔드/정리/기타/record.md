#record

[Java Record로 DTO를 만들어봅시다 — chaewss](https://chaewsscode.tistory.com/227#%F0%9F%98%BC%20DTO(%3A%20Data%20Transfer%20Object)-1)
[Java Record - Spring에서의 사용 사례와 함께](https://velog.io/@gongmeda/Java-Record-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0-Spring-%EC%97%90%EC%84%9C%EC%9D%98-Record-%EC%82%AC%EC%9A%A9-%EC%82%AC%EB%A1%80%EC%99%80-%ED%95%A8%EA%BB%98)

---

## 특징
1. **불변성** (Immutability)
record로 정의된 객체는 기본적으로 불변입니다. 즉, 생성 후에는 필드 값을 변경할 수 없습니다. 

2. 간결한 문법
필드, **생성자, getter를 자동으로 생성**할 수 있습니다. 코드가 간결해지고, 반복적인 작업을 줄일 수 있습니다.


3. 표준 메서드 자동 제공
**`equals()`, `hashCode()`, `toString()` 메서드가 자동으로 제공**되며, 서로 다른 record가 필드 값이 같으면 같은 객체로 인식한다.

4. annotation 사용 가능
**Builder,  Validation을 적용**시킬 수 있다.
```java
@Builder
public record SignupRequestDto(
    @NotNull @Email String username,
    @NotNull @Pattern(regexp = PASSWORD_REGEXP) String password,
    @NotNull @Pattern(regexp = NICKNAME_REGEXP) String nickname
) {}
```
