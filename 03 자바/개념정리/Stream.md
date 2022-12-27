[자바의 정석 - 스트림(Stream) | Integerous DevLog (ryan-han.com)](https://ryan-han.com/post/dev/java-stream/)
[스트림에 대해 (velog.io)](https://velog.io/@zion9948/%EC%8A%A4%ED%8A%B8%EB%A6%BC%EC%97%90-%EB%8C%80%ED%95%B4#collect)
[자바 - 스트림(Stream) (velog.io)](https://velog.io/@gmtmoney2357/%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%8A%B8%EB%A6%BCStream)
[Java 스트림 Stream (1) 총정리 | Eric Han's IT Blog (futurecreator.github.io)](https://futurecreator.github.io/2018/08/26/java-8-streams/)
[[Java] Stream API 연습문제 풀이 (5/5) - MangKyu's Diary (tistory.com)](https://mangkyu.tistory.com/116)

## 배경지식

## 함수형 프로그래밍 등장이유
명령형 프로그래밍을 기반으로 개발했던 개발자들은 개발하는 소프트웨어의 크기가 커짐에 따라, 복잡하게 엉켜있는 스파게티 코드를 유지보수하는 것이  매우 힘들다는 것을 깨닫게 되었다. 
그리고 이를 해결하기 위해 함수형 프로그래밍이라는 프로그래밍 패러다임에 관심을 갖게 되었다. 함수형 프로그래밍은 거의 모든 것을 순수 함수로 나누어 문제를 해결하는 기법으로, **작은 문제를 해결하기 위한 함수를 작성하여 가독성을 높이고 유지보수**를 용이하게 해준다.

## 명령형 프로그래밍과 함수형 프로그래밍의 차이는?
- 명령형 프로그래밍-> 어떻게(How) 할 건지를 설명하는 방식
```java
// 1 ~ 10까지의 값이 i에 할당된다 
for(int i = 1 ; i < 10; i++){ System.out.println(i); }
```
- 함수형 프로그래밍 -> 무엇(What)을 할 건지를 설명하는 방식
```java
process(10, print(num));
```

출처 : [[프로그래밍] 함수형 프로그래밍(Functional Programming) 이란? - MangKyu's Diary (tistory.com)](https://mangkyu.tistory.com/111)


## 코드로 보는 Stream 사용효과
- Stream 사용전
```java
String[] nameArr = {"IronMan", "Captain", "Hulk", "Thor"}; 
List<String> nameList = Arrays.asList(nameArr); 


Arrays.sort(nameArr); 
Collections.sort(nameList); 

for (String str: nameArr) { System.out.println(str); } 
for (String str : nameList) { System.out.println(str); }
```

- Stream 사용후
``` java
String[] nameArr = {"IronMan", "Captain", "Hulk", "Thor"};  
List<String> nameList = Arrays.asList(nameArr);  
  
Arrays.stream(nameArr).sorted().forEach(System.out::println);  
nameList.stream().sorted().forEach(System.out::println);
```

## Stream의 특징
- **1. 원본의 데이터를 변경하지 않는다.**
Stream API는 원본의 데이터를 조회하여 원본의 데이터가 아닌 별도의 요소들로 Stream을 생성한다. 정렬이나 필터링 등의 작업은 별도의 Stream 요소들에서 처리가 된다.

``` java
List<String> sortedList = nameStream.sorted()
							.collect(Collections.toList());
```

- **2. Stream은 일회용이다.**
Stream API는 일회용이기 때문에 한번 사용이 끝나면 재사용이 불가능하다. 만약 닫힌 Stream을 다시 사용한다면 IllegalStateException이 발생하게 된다.

```java
userStream.sorted().forEach(System.out::print);

// 스트림이 이미 사용되어 닫혔으므로 IllegalStateException 에러 발생
int count = userStream.count(); 
```

- **3. 내부 반복으로 작업을 처리한다.**
stream에서는 반복 문법을 메소드 내부에 숨기고 있기 때문에 간결한 코드의 작성이 가능하다. 

```java
nameStream.forEach(System.out::println);
```

## Stream API 생성하는 방법
**1. 생성하기**
-   Stream 객체를 생성하는 단계
Stream 연산을 하기 위해서는 먼저 Stream 객체를 생성해주어야 한다. 배열, 컬렉션, 임의의 수, 파일 등 거의 모든 것을 가지고 스트림을 생성할 수 있다. 
여기서 주의할 점은 연산이 끝나면 Stream이 닫히기 때문에, Stream이 닫혔을 경우 다시 Stream을 생성해야 한다는 것이다.

 **2. 가공하기**
-   원본의 데이터를 별도의 데이터로 가공하기 위한 중간 연산
-   연산 결과를 Stream으로 다시 반환하기 때문에 연속해서 중간 연산을 이어갈 수 있다.
 어떤 객체의 Stream을 원하는 형태로 처리할 수 있으며, 중간 연산의 반환값은 Stream이기 때문에 필요한 만큼 중간 연산을 연결하여 사용할 수 있다. 

 **3. 결과 만들기**
-   가공된 데이터로부터 원하는 결과를 만들기 위한 최종 연산
-   Stream의 요소들을 소모하면서 연산이 수행되기 때문에 1번만 처리가능하다.

**Stream 연산 예시 코드**
``` java
List<String> myList = Arrays.asList("a1", "a2", "b1", "c2", "c1");

myList
    .stream()							// 생성하기
    .filter(s -> s.startsWith("c"))		// 가공하기
    .map(String::toUpperCase)			// 가공하기
    .sorted()							// 가공하기
    .count();							// 결과만들기
```

## 1.생성하기
###  Collection의 Stream 생성
Collection 인터페이스를 구현한 객체들(List, Set, Map 등)은 모두 이 메소드를 이용해 Stream을 생성할 수 있다. stream()을 사용하면 해당 Collection의 객체를 소스로 하는 Stream을 반환한다.

``` java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> listStream = list.stream();
```

###  배열의 Stream 생성
배열의 원소들을 소스로하는 Stream을 생성하기 위해서는 Stream의 of 메소드 또는 Arrays의 stream 메소드를 사용하면 된다.
```java
// 배열로부터 스트림을 생성
Stream<String> stream = Stream.of("a", "b", "c"); //가변인자
Stream<String> stream = Stream.of(new String[] {"a", "b", "c"});

Stream<String> stream = Arrays.stream(new String[] {"a", "b", "c"});
Stream<String> stream = Arrays.stream(new String[] {"a", "b", "c"}, 0, 3); //end범위 포함 x
```

### 원시 Stream 생성

원시 자료형들을 사용하기 위한 특수한 종류의 Stream(IntStream, LongStream, DoubleStream) 들도 사용할 수 있으며, Intstream같은 경우 range()함수를 사용하여 기존의 for문을 대체할 수 있다.

``` java
// 4이상 10 이하의 숫자를 갖는 IntStream
IntStream stream = IntStream.range(4, 10);
```

## 2. 가공하기
### 필터링 - Filter
![](https://k.kakaocdn.net/dn/cosc60/btqUtpT0X7F/ARRrSuekHbueKHTeqIpiNK/img.png)

Filter는 Stream에서 조건에 맞는 데이터만을 정제하여 더 작은 컬렉션을 만들어내는 연산이다. boolean을 반환하는 람다식을 작성하여 filter 함수를 구현할 수 있다. 

ex) a가 들어간 문자열만을 포함하도록 필터링하는 코드
```java
List<String> list = Arrays.asList("a", "b", "c", "ab");   
  
list.stream()  
        .filter(name -> name.contains("a"))  
        .forEach(System.out::println);
```

출력
```
a
ab
```

### 데이터 변환 - Map 
![](https://k.kakaocdn.net/dn/50XYG/btqUto1SV5U/qhaKNo7HpSYKMiy15SJcgk/img.png)

Map은 기존의 Stream 요소들을 변환하여 새로운 Stream을 형성하는 연산이다. 저장된 값을 특정한 형태로 변환하는데 주로 사용함.

ex) list의 자료들을 모두 대문자로 변환하는 코드
```java
List<String> list = Arrays.asList("a", "b", "c", "ab");  
  
List<String> collect = list.stream()  
        .map(s -> s.toUpperCase())  
        .collect(Collectors.toList());  
  
System.out.println(collect);
```

출력
```
[A, B, C, AB]
```

### 정렬 - Sorted

Stream의 요소들을 정렬하기 위해서는 sorted를 사용해야 하며, 파라미터로 Comparator를 넘길 수도 있다. 

ex) 정렬 예제 코드
```java
List<String> list = Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");  
  
//오름차순  
List<String> collect = list.stream()  
        .sorted()  
        .collect(Collectors.toList());  
  
System.out.println(collect);  
  
//내림차순  
List<String> collect1 = list.stream()  
        .sorted(Comparator.reverseOrder())  
        .collect(Collectors.toList());  
System.out.println(collect1);  
  
//길이로 비교  
List<String> collect2 = list.stream()  
        .sorted(Comparator.comparing(String::length))  
        .collect(Collectors.toList());  
System.out.println(collect2);
```

출력
```
[Go, Groovy, Java, Python, Scala, Swift]
[Swift, Scala, Python, Java, Groovy, Go]
[Go, Java, Scala, Swift, Groovy, Python]
```


### 특정 연산 수행 - Peek
Stream의 요소들을 대상으로 Stream에 영향을 주지 않고 특정 연산을 수행하기 위한 peek 함수가 존재한다. '확인해본다'라는 뜻을 지닌 peek 단어처럼, peek 함수는 Stream의 각각의 요소들에 대해 특정 작업을 수행할 뿐 결과에 영향을 주지 않는다. 

ex) 어떤 stream의 요소들을 중간에 출력하기를 원할 때 다음과 같이 활용할 수 있다.
```java
int sum = IntStream.of(1, 3, 5, 7, 9)  
        .peek(System.out::println)  
        .sum();  
System.out.println(sum);
```

출력
```
1
3
5
7
9
25
```

### 원시 Stream <-> Stream 
일반적인 Stream 객체는 mapToInt(), mapToLong(), mapToDouble()이라는 특수한 Mapping 연산을 지원하고 있으며, 그 반대로 원시객체는 mapToObject를 통해 일반적인 Stream 객체로 바꿀 수 있다.
```java
// IntStream -> Stream<Integer>  
List<String> collect1 = IntStream.range(1, 4)  
        .mapToObj(i -> "a" + i)  
        .collect(Collectors.toList());  
  
System.out.println(collect1);  
  
// Stream<Double> -> IntStream -> Stream<String>  
String collect2 = Stream.of(1.0, 2.0, 3.0)  
        .mapToInt(Double::intValue)  
        .mapToObj(i -> "a" + i + i + " ")  
        .collect(Collectors.joining());  
  
System.out.println(collect2);
```

출력
```
[a1, a2, a3]
a11 a22 a33 
```


### 중복 제거 - Distinct 
Stream의 요소들에 중복된 데이터가 존재하는 경우, 중복을 제거하기 위해 distinct를 사용할 수 있다. 

ex) 중복 제거 예시
```java
List<String> list2 = Arrays.asList("A", "B", "A", "C", "C");  
  
//중복제거  
List<String> collect = list2.stream()  
        .distinct()  
        .collect(Collectors.toList());  
  
System.out.println(collect);
```

출력
```
[A, B, C]
```

🧨자료가 객체인 경우에는 equals와 hashCode를 overriding해야함.