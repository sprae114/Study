해시태크 : #enum
참고자료
1) [다형성을 사용해 if/else를 제거하는 리팩토링을 하라. :: SLiPP](https://www.slipp.net/questions/566)
2) [[java] Java에서 열거 형을 반복하는 'for'루프 - 리뷰나라 (daplus.net)](http://daplus.net/java-java%EC%97%90%EC%84%9C-%EC%97%B4%EA%B1%B0-%ED%98%95%EC%9D%84-%EB%B0%98%EB%B3%B5%ED%95%98%EB%8A%94-for%EB%A3%A8%ED%94%84/)
3) [Java Enum 1편 : Enum 기본적인 사용 :: 뱀귤 블로그 (tistory.com)](https://bcp0109.tistory.com/334)
4) [Java Enum 활용기 | 우아한형제들 기술블로그 (woowahan.com)](https://techblog.woowahan.com/2527/)
5) [Enum 활용사례 3가지 (tistory.com)](https://jojoldu.tistory.com/137)

----

## 언제 enum을 써야되는가?
- 자료에 "남자" 또는 "여자" 와 같이 한정된 자료만을 입력하도록 지정하고 싶을 때
	기존에는 타입을 제한 할 수 있지만 값을 제한할 순 없었다.
- **동일 타입 + 제한된 자료 입력**


## 특징
- new 연산자를 사용하지 않고 '**타입명.속성명' 으로 호출**함(static 특성)
- **== 으로 객체를 비교**할 수 있다
- **enum 속성값은 변경불가** (final 특성)

## 사용방법
#### 📌타입명만 이용하는 경우
```java
public class TypeA {  
   /** enum 타입의 정의 */  
   public enum GenderType {  
      MAN, WOMAN  
   };  
  
   /** GenderType 전역변수 생성 */  
   private GenderType genderType;  
  
   /** genderType getter 함수 생성 */  
   public GenderType getGenderType() {  
      return genderType;  
   }  
  
   /** genderType setter 함수 생성 */  
   public void setGenderType(GenderType genderType) {  
      this.genderType = genderType;  
   }  
}
```


- 실행코드 예시
```java
public class TestMain {  
   public static void main(String[] args) {  
      TypeA a = new TypeA();  
  
      /** 자료의 입력은 『타입.속성명』 형태로 입력을 한다. */  
      a.setGenderType(GenderType.MAN);  
  
      /** 비교는 연산자 『==』로 할 수 있다. */  
      if (a.getGenderType() == GenderType.MAN) {  
         System.out.println("객체비교 a.getGenderType() [남자]");  
      } else if (a.getGenderType() == GenderType.WOMAN) {  
         System.out.println("객체비교 a.getGenderType() [여자]");  
      } else {  
         System.out.println("비교불가");  
      }  
   }  
}
```

#### 📌타입+속성을 이용하는 경우
```java
public enum Level {  
    LEVEL1("레벨1"),  
    LEVEL2("레벨2"),  
    LEVEL3("레벨3"),  
    LEVEL4("레벨4"),  
    LEVEL5("레벨5");  
  
    private String name;  
  
    Level(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
}


public enum Mission {  
    CAR("레벨1", "자동차경주"),  
    LOTTO("레벨1", "로또"),  
    BASEBALL("레벨1", "숫자야구게임"),  
    BASKET("레벨2", "장바구니"),  
    PAYMENT("레벨2", "결제"),  
    SUBWAY("레벨2", "지하철노선도"),  
    IMPROVEMENT("레벨4", "성능개선"),  
    DISTRIBUTION("레벨4", "배포");  
  
    String level;  
    String name;  
  
    Mission(String level, String name) {  
        this.level = level;  
        this.name = name;  
    }  
  
    public String getLevel() {  
        return level;  
    }  
  
    public String getName() {  
        return name;  
    }  
}
```


- 실행
```java
public class Main {  
    public static void main(String[] args) {  
        for (Level s : Level.values()){  
            List<String> collect = Arrays.stream(Mission.values())  
                    .filter(mission -> mission.getLevel().equals(s.getName()))  
                    .sequential()  
                    .map(Mission::getName)  
                    .collect(Collectors.toList());  
            System.out.println(collect);  
        }  
    }  
}
```

- 출력
```
[자동차경주, 로또, 숫자야구게임]
[장바구니, 결제, 지하철노선도]
[]
[성능개선, 배포]
[]
```