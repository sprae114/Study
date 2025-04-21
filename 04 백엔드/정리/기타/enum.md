#enum

1) [다형성을 사용해 if/else를 제거하는 리팩토링을 하라.](https://www.slipp.net/questions/566)
2) [[java] Java에서 열거 형을 반복하는 'for'루프 - 리뷰나라 (daplus.net)](http://daplus.net/java-java%EC%97%90%EC%84%9C-%EC%97%B4%EA%B1%B0-%ED%98%95%EC%9D%84-%EB%B0%98%EB%B3%B5%ED%95%98%EB%8A%94-for%EB%A3%A8%ED%94%84/)
3) [Java Enum 1편 : Enum 기본적인 사용](https://bcp0109.tistory.com/334)
4) [Java Enum 활용기 | 우아한형제들 기술블로그 (woowahan.com)](https://techblog.woowahan.com/2527/)
5) [Enum 활용사례 3가지 (tistory.com)](https://jojoldu.tistory.com/137)

----

## 언제 enum을 써야되는가?
- 자료에 "남자" 또는 "여자" 와 같이 한정된 자료만을 입력하도록 지정하고 싶을 때 사용함. 기존에는 타입을 제한 할 수 있지만 값을 제한할 순 없었다.
- **동일 타입 + 제한된 자료 입력**


## 특징
- new 연산자를 사용하지 않고 **`타입명.속성명` 으로 호출**함(static 특성)
- **== 으로 객체를 비교**할 수 있다
- **enum 속성값은 변경불가** (final 특성)


## 사용방법
#### 타입명
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

#### 타입+속성
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

# Enum과 엔티티
[JPA 엔티티에서 Enum을 더 유연하게 사용하는 방법 (@Converter)](https://velog.io/@calaf/JPA-%EC%97%94%ED%8B%B0%ED%8B%B0%EC%97%90%EC%84%9C-Enum%EC%9D%84-%EB%8D%94-%EC%9C%A0%EC%97%B0%ED%95%98%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-Converter)
## 타입명

```java
public enum Color {
	RED,
    BLUE,
    YELLOW,
    ORANGE
}
```

```java
@Entity
public class FooEntity {
    @Id
    private Long id;

	@Enumerated(EnumType.STRING)
    private Color color;
}
```

## 타입명 + 속성
### 설명
- `AttributeConverter 인터페이스`
```java
public interface AttributeConverter<X,Y> {
	//엔티티의 속성을 데이터베이스 컬럼에 저장할 수 있는 형태로 변환합니다.
	public Y convertToDatabaseColumn (X attribute);

	//데이터베이스에서 읽어온 값을 엔티티 속성의 형태로 변환합니다.
	public X convertToEntityAttribute (Y dbData);
}
```


### 코드
```java
@Getter
@AllArgsConstructor
public enum Color {
    RED("빨간색"),
    YELLOW("노란색"),
    ORANGE("오렌지색");

    private final String name;

    public static Color getInstance(String name){
        return Arrays.stream(values())
                .filter(color -> color.getName().equals(name))
                .findFirst()
                .orElseThrow();
    }
}
```

```java
import jakarta.persistence.AttributeConverter;  
import jakarta.persistence.Converter;

@Converter
public class ColorConverter implements AttributeConverter<Color, String> {

	// 엔티티 -> 칼럼
    @Override
    public String convertToDatabaseColumn(Color attribute) {
        if (attribute == null){
            return null;
        }
        return attribute.getName();
    }

	// 칼럼 -> 엔티티
    @Override
    public Color convertToEntityAttribute(String dbData) {
        return Color.getInstance(dbData);
    }
}
```

```java
@Entity
@AllArgsConstructor
public class FooEntity {
    @Id
    private Long id;
    
    @Convert(converter = ColorConverter.class)
    private Color color;
}
```
- `@Convert(converter = ColorConverter.class)` :이 필드를 데이터베이스에 저장하고 읽어올 때 변환 작업을 수행하도록 지정합니다.

## enum 조회
- 두 코드의 차이는?
```java
SchoolName findSchoolName = Arrays.stream(SchoolName.values())  
        .filter(school -> school.getSchoolName().equals(schoolName))  
        .findAny()  
        .orElseThrow(IllegalArgumentException::new);

SchoolName findSchoolName = SchoolName.valueOf(schoolName)
```