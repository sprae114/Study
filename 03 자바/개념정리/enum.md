[다형성을 사용해 if/else를 제거하는 리팩토링을 하라. :: SLiPP](https://www.slipp.net/questions/566)


## 언제 enum을 써야되는가?
- 자료에 "남자" 또는 "여자" 와 같이 한정된 자료만을 입력하도록 지정하고 싶을 때
	기존에는 타입을 제한 할 수 있지만 값을 제한할 순 없었다.

- **동일 타입 + 제한된 자료 입력**

## 정의 및 사용방법
1) 정의방법
`enum 타입명 {속성1, 속성2, ...}`


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


2) 사용방법

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

## 특징
- new 연산자를 사용하지 않고 '타입명.속성명' 으로 호출함(static 특성)
- == 으로 객체를 비교할 수 있다
- enum 속성값은 변경불가 (final 특성)

## 타입의 속성값 설정


## enum for문
[[java] Java에서 열거 형을 반복하는 'for'루프 - 리뷰나라 (daplus.net)](http://daplus.net/java-java%EC%97%90%EC%84%9C-%EC%97%B4%EA%B1%B0-%ED%98%95%EC%9D%84-%EB%B0%98%EB%B3%B5%ED%95%98%EB%8A%94-for%EB%A3%A8%ED%94%84/)
[Java Enum 1편 : Enum 기본적인 사용 :: 뱀귤 블로그 (tistory.com)](https://bcp0109.tistory.com/334)

[Java Enum 활용기 | 우아한형제들 기술블로그 (woowahan.com)](https://techblog.woowahan.com/2527/)
[Enum 활용사례 3가지 (tistory.com)](https://jojoldu.tistory.com/137)
