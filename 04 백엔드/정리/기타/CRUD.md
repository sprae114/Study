# Create


# Read


# Update
## dirty checking
[[Spring] JPA 변경감지와, 병합을 통한 Update](https://wooj-coding-fordeveloper.tistory.com/77?category=1079495)
[더티 체킹 (Dirty Checking)이란?](https://jojoldu.tistory.com/415)


## @Setter
[@Builder 패턴으로 객체 수정시 값이 초기화 되는 문제](https://velog.io/@shdrnrhd113/Builder-%ED%8C%A8%ED%84%B4%EC%9C%BC%EB%A1%9C-%EA%B0%9D%EC%B2%B4-%EC%88%98%EC%A0%95%EC%8B%9C-%EA%B0%92%EC%9D%B4-%EC%B4%88%EA%B8%B0%ED%99%94-%EB%90%98%EB%8A%94-%EB%AC%B8%EC%A0%9C#-service-%EA%B3%84%EC%B8%B5---%EC%88%98%EC%A0%95-%EB%A1%9C%EC%A7%81)
[setter를 지양하고 Builder 패턴을 사용하자](https://skatpdnjs.tistory.com/13)

### @Setter의 문제점은?
1. 의도를 파악하기 힘듦
```java
member.setFistName("value");
member.setLastName("value");
member.setAge("value");
```
단순히 setter로 값을 변경한 코드다. 코드를 봤을 땐 왜 값을 변경하게 되었는지 알려면 단순히 생각해내는 것 말고는 코드를 역추적해서 찾을 수 밖에 없다.

2. 객체의 일관성을 유지하기 위함
회원의 이름을 변경해야할 때가 오면 무작정 setter로 값을 바꾸게 되면 회원의 이름을 변경하는 메서드가 의미가 없게 된다. 그리고 나중에 찾기도 힘듦.

### 해결책은?
빌더패턴을 사용하여 안정성(생성자의 장점)과 자바빈즈의 가독성(setter의 장점)을 가져가자.
```java
@Entity
@Builder(toBuilder = true)
public class Comment {
	....
}
```


```java
public void modify(Comment comment, String contents) {
    Comment comment1 = comment.toBuilder()   // toBuilder 로 수정!!!
            .content(contents)
            .modifyDate(LocalDateTime.now())
            .build();
            
    this.commentRepository.save(comment1);
}
```
- `builder()`가 아닌 `toBuilder()`를 사용하여 기존 객체에 덮어씌운다.


# Delete
