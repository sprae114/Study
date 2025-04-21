## Spring AOP와 프록시
#### 문제
```java
@Service
public class UserService {
    @Transactional
    public void methodA() {
        // 트랜잭션 내에서 수행할 작업
    }

    public void methodB() {
        this.methodA(); // 트랜잭션이 적용되지 않음
    }
}
```

#### 이유
A 메서드의 트랜잭션이 적용되지 않는 이유는 **Spring AOP**의 프록시 기반 트랜잭션 관리 방식 때문입니다.

Spring은 AOP를 사용하여 `@Transactional`이 적용된 **메서드를 감싸는 프록시 객체를 생성**합니다. 이 프록시는 메서드가 호출될 때 트랜잭션을 시작하고, 메서드 실행 후에 커밋하거나 롤백합니다.

**같은 클래스 내에서 `this.A()`를 호출하면, 실제로 프록시 객체가 아닌 현재 객체(즉, `this`)의 메서드를 호출**하게 됩니다. 이 경우, A 메서드는 프록시가 아닌 실제 객체의 메서드가 호출되므로, 트랜잭션이 적용되지 않습니다.


#### 해결 방법
호출할 때 `@Autowired`로 주입한 다른 서비스 클래스의 메서드를 호출합니다.
```java
@Service
public class UserService {
    @Autowired
    private UserService userService; // 다른 서비스처럼 주입

    @Transactional
    public void methodA() {
        // 트랜잭션 내에서 수행할 작업
    }

    public void methodB() {
        userService.methodA(); // 트랜잭션이 적용됨
    }
}
```