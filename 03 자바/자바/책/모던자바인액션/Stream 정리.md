
#스트림, #Stream, #복습자바

1) [자바의 정석 - 스트림(Stream) | Integerous DevLog (ryan-han.com)](https://ryan-han.com/post/dev/java-stream/)
2) [Java 스트림 Stream (1) 총정리 | Eric Han's IT Blog (futurecreator.github.io)](https://futurecreator.github.io/2018/08/26/java-8-streams/)
3) [[Java] Stream API 연습문제 풀이 (5/5) - MangKyu's Diary (tistory.com)](https://mangkyu.tistory.com/116)
4) 책 : 모던 자바 인 액션
----
# <코드> 스트림 시작 연산
java/javaPractice/src/ModernJava/chap04
## 1. String 
#### chars ✔
▶ 개념
`chars()` 메소드를 사용하면, **문자열의 각 문자(정확히는 해당 문자의 유니코드 값)를 요소로 하는 `IntStream`** 을 얻을 수 있습니다. 

▶ 예시
ex) 각각의 알파벳을 `Character`로 리스트로 담기
```java
String str = "hello world";  

List<Character> collect = str.chars()  
        .mapToObj(s -> (char) s)  
        .collect(Collectors.toList());  
  
System.out.println(collect);  
// [h, e, l, l, o,  , w, o, r, l, d]
```

ex) 각각의 알파벳의 `Integer`로 리스트로 담기
```java
String str = "hello world";  

List<Integer> collect1 = str.chars()  
        .boxed() // .mapToObj(s -> (Integer) s) 와 같음  
        .collect(Collectors.toList());  
  
System.out.println(collect1);
// [104, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100]
```

ex) 특정문자 필터링 스트림  
```java
String str2 = "abc 123!@#$";

List<Character> collect11 = str2.chars()  
        .filter(s1 -> s1 < 90 && s1 > 50)  
        .mapToObj(n -> (char) n)  
        .collect(Collectors.toList());  
        
System.out.println(collect11); // [3, @]
```

#### split ✔
ex) 각각의 알파벳을 `String`로 리스트로 담기
```java
String str = "hello world";  

List<String> collect2 = Arrays.stream(str.split(""))  
        .collect(Collectors.toList());  
  
System.out.println(collect2);  
// [h, e, l, l, o,  , w, o, r, l, d]
```

ex) 띄어쓰기 기준으로 리스트 담기
```java
String str = "hello world";  

List<String> collect3 = Arrays.stream(str.split(" "))  
        .collect(Collectors.toList());  
  
System.out.println(collect3);
// [hello, world]
```


## 2. Array 
#### Arrays.stream ✔
배열을 스트림으로 변환하려면 **`Arrays.stream()` 메소드를 사용**하면 됩니다.
ex) `int[]` -> 리스트 변환
```java
int[] array = {1, 2, 3, 4, 5};  

List<Integer> collect = Arrays.stream(array)  
        .boxed()  // int -> integer 로 변환하기 위함.
        .collect(Collectors.toList());  
  
System.out.println(collect);  
```

ex) `String[]` -> 리스트 변환
```java
String[] array2 = {"a", "b", "c", "d"};  
  
List<String> collect1 = Arrays.stream(array2)  
        .collect(Collectors.toList());  
  
System.out.println(collect1);
```


## 3. Collection (List, Set, Map 등)
#### Stream ✔
Collection 인터페이스에는 `stream()` 메소드가 정의되어 있으므로, `List`나 `Set` 등의 **`Collection` 객체를 바로 스트림으로 변환**할 수 있습니다.
▶ List
ex) `a`가 포함된 문자만 리스트로 담기
```java
List<String> list = Arrays.asList("apple", "banana", "cherry");  
  
List<String> collect = list.stream()  
        .filter(s -> s.contains("a"))  
        .collect(Collectors.toList());  
  
System.out.println(collect); //[apple, banana]
```

▶ Map
Map 객체도 스트림으로 변환할 수 있지만, 이 경우에는 `key set`, `value set` 또는 `entry set`중 하나를 선택하여 스트림으로 만들어야 합니다.
```java
Map<String, Integer> map = Map.ofEntries(  
        Map.entry("apple", 1),  
        Map.entry("banana", 2),  
        Map.entry("cherry", 3)  
);  
  
// Key를 리스트에 담기
List<String> collect = map.keySet()  
        .stream()  
        .filter(s -> s.contains("a"))  
        .collect(Collectors.toList());  
  
System.out.println(collect);  // [apple, banana]
  
// Value를 리스트에 담기
List<Integer> collect1 = map.values()  
        .stream()  
        .map(n -> n * n)  
        .collect(Collectors.toList());  // [1, 9, 4]
  
System.out.println(collect1);  
  
// Map을 리스트에 담기
List<Map.Entry<String, Integer>> collect2 = map.entrySet()  
        .stream()  
        .collect(Collectors.toList()); // [apple=1, cherry=3, banana=2]
```


▶ Set
ex) 짝수인 것만 리스트에 담기
```java
Set<Integer> set = new HashSet<>(Set.of(1,2,3,4,5));  
  
List<Integer> collect4 = set.stream()  
        .filter(n -> n % 2 == 0)  
        .collect(Collectors.toList());  
  
System.out.println(collect4); // [2, 4]
```


## 4. 숫자
#### Stream.iterate✔
`iterate` 메서드는 **초기값과 갱신 함수를 기반**으로 시퀀셜 스트림을 생성합니다. Java 9부터는 종료 조건을 추가하여 유한 스트림을 생성할 수 있습니다.

```java
// Java 8 스타일: 무한 스트림 생성
<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f);

// Java 9 스타일: 유한 스트림 생성
<T> Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next);
```
- `seed` : 초기값 설정
- `UnaryOperator` : 갱신 함수 설정
- `hasNext` : 종료조건 설정 (🧨2번째 변수임)


ex) 0부터 2씩 더해서 5개 출력하기
```java
List<Integer> collect = Stream.iterate(0, n -> n + 2)  
        .limit(5)  
        .collect(Collectors.toList());  
  
System.out.println(collect); // [0, 2, 4, 6, 8]
```


ex) 0부터 3씩 더하거 + 짝수만 필터링 5개 출력하기
```java
List<Integer> collect1 = IntStream.iterate(0, n -> n + 3)  
        .filter(n -> n % 2 == 0) // 짝수만 필터링  
        .limit(5) // 스트림 요소를 5개로 제한  
        .boxed()  
        .collect(Collectors.toList());  
  
System.out.println(collect1); // [0, 6, 12, 18, 24]
```


ex) 피보나치 수열 만들기
```java
 // 피보나치 수열을 리스트로 생성
List<Long> fibonacciList = Stream.iterate(new long[]{0, 1}, 
										fib -> new long[]{fib[1], fib[0] + fib[1]})
	.limit(10) // 첫 10개의 피보나치 수열을 제한
	.map(fib -> fib[0]) // 각 배열의 첫 번째 요소를 Long 타입으로 변환
	.collect(Collectors.toList()); // 리스트로 수집

System.out.println(fibonacciList); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```
- `iterate` : 람다 표현식에 의해 다음과 같이 생성됩니다. `[0, 1]`, `[1, 1]`, `[1, 2]`, .... 
- `map` : 각 배열의 첫 번째 요소를 스트림의 요소로 변환됨. `0, 1, 1, 2, ... `


ex)  0부터 3씩 더해서 20미만 출력 + 조건을 이용
```java
List<Integer> collect2 = Stream.iterate(0, n -> n < 20, n -> n + 3)  
        .collect(Collectors.toList());  
  
System.out.println(collect2); // [0, 3, 6, 9, 12, 15, 18]  
  
  
List<Integer> collect3 = IntStream.iterate(0, n -> n < 20, n -> n + 3)  
        .boxed()  
        .collect(Collectors.toList());  
  
System.out.println(collect3); // [0, 3, 6, 9, 12, 15, 18]
```

ex)  알파벳 a ~ j 까지 출력
```java
List<Character> charList = Stream.iterate('a',  
                c -> c <= 'j',  
                c -> (char)(c + 1))  
        .collect(Collectors.toList());  
  
System.out.println(charList); // [a, b, c, d, e, f, g, h, i, j]
```


#### IntStream.rangeClosed ✔
▶ 개념
시작값과 끝값을 포함하여 **일정 범위 내의 숫자 스트림을 생성**하는 데 사용됩니다. 이는 연속된 정수 스트림을 생성할 때 유용합니다.

▶ 어떻게 사용하는지
```java
IntStream rangeClosed(int startInclusive, int endInclusive);
IntStream range(int startInclusive, int endInclusive);
```

▶ 예시
ex) 1 ~ 10까지 짝수 출력
```java
List<Integer> collect1 = IntStream.rangeClosed(1, 10)  
        .filter(n -> n % 2 == 0)  
        .boxed()  
        .collect(Collectors.toList());  
  
System.out.println(collect1);  // [2, 4, 6, 8, 10]
  
List<Integer> collect2 = IntStream.range(1, 12)  
        .filter(n -> n % 2 == 0)  
        .boxed()  
        .collect(Collectors.toList());  
  
System.out.println(collect2); // [2, 4, 6, 8, 10]
```

ex) 두 개의 주사위를 굴려서 나온 눈의 합이 6인 경우를 모두 출력
```java
List<Integer[]> collect = IntStream.rangeClosed(1, 6)  
        .boxed()  
        .flatMap(n1 -> IntStream.rangeClosed(1, 6).boxed().map(n2 -> new Integer[]{n1, n2}))  
        .filter(array -> array[0] + array[1] == 6)  
        .collect(Collectors.toList());  
  
collect.forEach(line -> System.out.println(Arrays.toString(line)));
//[1, 5]  
//[2, 4]  
//[3, 3]  
//[4, 2]  
//[5, 1]
```


#### generate
ex) 임의의 double 스트림
```java
List<Double> collect = Stream.generate(Math::random)  
        .limit(3)  
        .collect(Collectors.toList());  
        
System.out.println(collect);
// [0.4383143801392815, 0.7571514993934333, 0.14408242014275896]
```


ex) 요소 1을 갖는 스트림  
```java
String collect1 = IntStream.generate(() -> 1)  
        .limit(3)  
        .mapToObj(String::valueOf)  
        .collect(Collectors.joining(" "));  
  
System.out.println(collect1); // 1 1 1
```

## 5. 기타
#### Stream.of ✔
임의의 수를 인수로 받는 **정적 메서드 `Stream.of`를 이용해서 스트림**을 만들 수 있음.

ex) 개별 요소로 스트림 생성
```java
List<String> collect1 = Stream.of("apple", "banana", "cherry")  
        .collect(Collectors.toList());  
  
System.out.println(collect1); // [apple, banana, cherry]
```

ex) 빈 스트림 생성
```java
long count = Stream.of().count();  
System.out.println(count); // 0
```


#### Files
`java.nio.file.Files` 클래스의 `lines()` 메소드를 사용하면 텍스트 파일의 각줄을 요소로 하는 스트림을 생성할 수 있습니다.
```java
try {
	Stream<String> lines = Files.lines(Paths.get("file.txt"));
	// do something with the stream...
} catch (IOException e) {
	e.printStackTrace();
}
```


# <코드> 중간 연산
java/javaPractice/src/ModernJava/chap05
## 사용되는 데이터
```java
public Dish(String name, boolean vegetarian, int calories, Type type) {  
  this.name = name;  
  this.vegetarian = vegetarian;  
  this.calories = calories;  
  this.type = type;  
}

public static final List<Dish> menu = Arrays.asList(  
    new Dish("pork", false, 800, Type.MEAT),  
    new Dish("beef", false, 700, Type.MEAT),  
    new Dish("chicken", false, 400, Type.MEAT),  
    new Dish("french fries", true, 530, Type.OTHER),  
    new Dish("rice", true, 350, Type.OTHER),  
    new Dish("season fruit", true, 120, Type.OTHER),  
    new Dish("pizza", true, 550, Type.OTHER),  
    new Dish("prawns", false, 400, Type.FISH),  
    new Dish("salmon", false, 450, Type.FISH)
```


## 1. 필터링
스트림에서 **요소를 선택**하는 필터링을 제공

#### Filter ✔
인수로 받아서 프레디케이트와 **일치하는 모든 요소를 포함하는 스트림을 반환.**
![[Pasted image 20230906100700.png]]

ex) vegetarian이 true인 것만 리스트에 넣기
```java
List<Dish> vegetarianMenu = menu.stream()  
        .filter(Dish::isVegetarian)  
        .collect(toList());  
  
System.out.println(vegetarianMenu); // [french fries, rice, season fruit, pizza]
```


ex) 이름이 'A' 또는 'E'로 시작하고 길이가 3보다 큰 문자열을 필터링
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");  
  
List<String> filteredNames = names.stream()  
        .filter(name -> name.startsWith("A") || name.startsWith("E"))  
        .filter(name -> name.length() > 3)  
        .collect(Collectors.toList());  
  
System.out.println(filteredNames); // [Alice]
```


#### Distinct ✔
**요소의 중복을 제거후 반환**. (고유 여부는 스트림에서 만든 객체의 hashCode, equals로 결정된다.)
![[Pasted image 20221117180302.png]]

ex) 짝수인 것만 중복 제거해서 출력
```java
List<Integer> numbers = Arrays.asList(1, 4, 1, 3, 3, 2, 2);  
  
String collect = numbers.stream()  
        .filter(i -> i % 2 == 0)  
        .distinct()  
        .map(String::valueOf)  
        .collect(Collectors.joining(" "));  
  
System.out.println(collect); // 4 2
```


## 2. 제한
스트림의 요소를 **선택**하거나 **스킵**하는 것

#### takeWhile / dropWhile
**이미 정렬된 리스트/배열일 경우, 조건에 벗어날때  반복작업을 중단**하는 방법
why? 큰 배열일 경우 조건에 맞지않는 그 순간 Stream 조회를 종료하는게 효과적임.

- **takeWhile** 
스트림의 **시작 부분부터 주어진 조건을 만족하는 동안** 요소를 가져옵니다
![[Pasted image 20230906100710.png]]

ex) 정렬된 menu에서 칼로리 320작은 것 찾기
```java
menu.stream()  
        .takeWhile(dish -> dish.getCalories() < 450)  
        .forEach(System.out::println);
```

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
// 5 이하의 숫자들만 취함
List<Integer> takenNumbers = numbers.stream()
	.takeWhile(n -> n <= 5)
	.collect(Collectors.toList());

System.out.println(takenNumbers); // [1, 2, 3, 4, 5]

// 짝수인 동안 숫자를 취함
List<Integer> takenEvenNumbers = numbers.stream()
	.takeWhile(n -> n % 2 == 0)
	.collect(Collectors.toList());

System.out.println(takenEvenNumbers); // []
```


- **DROPWHILE**
처음으로 조건을 만족하지 않는 요소가 나타나면 그 시점부터 모든 나머지 요소들(조건과 상관없이)이 포함된 새로운 스트림을 반환합니다.  (**takeWhile와 반대**)
![[Pasted image 20230906100725.png]]

```java
menu.stream()  
        .dropWhile(dish -> dish.getCalories() < 450)  
        .forEach(System.out::println);
```

출력
```
pork
beef
chicken
french fries
rice
season fruit
pizza
prawns
salmon
```


#### Limit ✔
**주어진 값 이하의 크기**를 가지는 새로운 스트림을 반환

ex) 300칼로리 이상의 세 요리를 리스트에 넣어라
```java
List<Dish> dishesLimit3 = menu.stream()  
    .filter(d -> d.getCalories() > 300)  
    .limit(3)  
    .collect(toList());  
dishesLimit3.forEach(System.out::println);
```

출력
```
pork
beef
chicken
```


#### Skip ✔
**처음  n 개의 요소를 제외**한(스킵한) 스트림을 반환
![[Pasted image 20221117183329.png]]

ex) 300칼로리 이상의 처음 두 요리를 건너띈 다음 나머지 요리 반환
```java
List<Dish> dishesSkip2 = menu.stream()  
    .filter(d -> d.getCalories() > 300)  
    .skip(2)  
    .collect(toList());  
dishesSkip2.forEach(System.out::println);
```

출력
```
chicken
french fries
rice
pizza
prawns
salmon
```


## 3. 변환
특정 객체에서 **특정 데이터를 처리**하는 작업

#### map ✔
인수로 제공된  함수는 각 요소에 적용되며 **함수를 적용한 결과가 새로운 요소로 매핑**
![[Pasted image 20230906100739.png]]

ex) 요리명을 추출하는 예제 
```java
List<String> dishNames = menu.stream()  
    .map(Dish::getName)  
    .collect(toList());  
    
System.out.println(dishNames);  
// [pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon]
```

ex) map을 연결하여 요리명의 글자수 추출
```java
// map을 연결하여 요리명의 글자수 추출
List<Integer> dishNamesLengths = menu.stream()  
        .map(Dish::getName)  
        .map(String::length)  
        .collect(toList());  
        
System.out.println(dishNamesLengths);  
// [4, 4, 7, 12, 4, 12, 5, 6, 6]
```


#### mapToInt ✔
▶ 개념
`mapToInt` 메서드는 Java Stream API에서 **객체 스트림(`Stream<T>`)의 요소를 `int` 값으로 매핑하여 `IntStream`으로 변환할 때 사용**됩니다. 이를 통해 객체 스트림의 요소를 정수형으로 변환하여 **다양한 연산(`sum`, `max/min`, `count`, `average`)을 수행**할 수 있습니다.

▶ 어떻게 사용하는지
```java
IntStream mapToInt(ToIntFunction<? super T> mapper);
```

▶ 예시
reduce로 하면 코드가 좀더 복잡하짐 + Integer에서 int로 전환하는 박싱비용 소모된다. 그래서 **기본형에 특화된 스트림을 제공**함.
``` java
//기존코드  
int calories = menu.stream()  
        .map(Dish::getCalories)  
        .reduce(0, (a, b) -> a + b);   
        //.reduce(0, Integer::sum) 과 같음  
  
System.out.println(calories); // 4300  
  
//바뀐코드  
int calories2 = menu.stream()  
        .mapToInt(Dish::getCalories)  
        .sum();   
  
System.out.println(calories2); // 4300
```

ex) 자료구조 저장할때는 래퍼 클래스로 저장해야함.
```java
List<Integer> collect = menu.stream()  
        .map(Dish::getCalories)  
        .collect(toList());  
  
List<Integer> collect3 = menu.stream()  
        .mapToInt(Dish::getCalories)  
        .boxed()  // int 값이라 박싱비용
        .collect(toList());
```

ex) 평균 구하기
```java
double average = menu.stream()  
        .mapToInt(Dish::getCalories)  
        .average()  
        .orElse(0L);  
  
System.out.println(average);
```


#### mapToObj ✔
▶ 개념
`mapToObj` 메서드는 Java Stream API에서 **기본형 스트림(예: `IntStream`, `LongStream`, `DoubleStream`)을 객체 스트림(`Stream<T>`)으로 변환할 때 사용**됩니다. 기본형 스트림의 요소를 객체로 매핑하여, 객체 스트림으로 변환할 수 있습니다.


▶ 어떻게 사용하는지
```java
<R> Stream<R> mapToObj(IntFunction<? extends R> mapper);
<R> Stream<R> mapToObj(LongFunction<? extends R> mapper);
<R> Stream<R> mapToObj(DoubleFunction<? extends R> mapper);
```

▶ 예시
ex) 칼로리 숫자를 문자열로 표현하기
```java
// 기존 코드
String collect5 = menu.stream()  
        .map(Dish::getCalories)  
        .map(String::valueOf)  
        .collect(Collectors.joining(" "));  
  
System.out.println(collect5); // 800 700 400 530 350 120 550 400 450  

// 바뀐 코드
String collect6 = menu.stream()  
        .mapToInt(Dish::getCalories)  
        .mapToObj(String::valueOf)  
        .collect(Collectors.joining(" "));  
  
System.out.println(collect6); // 800 700 400 530 350 120 550 400 450
```


#### FlatMap ✔
![[Pasted image 20221117203513.png]]

ex) `[Hello, World]` -> `[H, e, l, o, W, r, d]`로 만들기
```java
List<String> words = Arrays.asList("Hello", "World");

List<String> collect1 = words.stream()  
        .flatMap((String line) -> Arrays.stream(line.split("")))  
        // ["Hello", "World"] 
		//-> [["H", "e", "l", "l", "o"], ["W", "o", "r", "l", "d"]]
		//-> ["H", "e", "l", "l", "o", "W", "o", "r", "l", "d"]
        .distinct()  
        .collect(toList());  
        
System.out.println(collect1);  


List<String> collect2 = words.stream()  
        .map(s -> s.split(""))   //각 문자를 포함하는 배열로 반환  
        .flatMap(Arrays::stream)  //하나의 스트림으로 평면화  
        .distinct()  
        .collect(toList());  
        
System.out.println(collect2);  
```

if) map을 쓴다면 어떻게 결과가 나올까?
Hello와 world의 중복검사를 함.
![[Pasted image 20221117202741.png]]

ex) 2차 리스트 만들기
```java
List<List<String>> nestedList = Arrays.asList(
            Arrays.asList("a", "b", "c"),
            Arrays.asList("d", "e", "f"),
            Arrays.asList("g", "h", "i")
        );


// 중첩된 리스트를 평탄화
List<String> flatList = nestedList.stream()
	.flatMap(List::stream) 
	// [["a", "b", "c"], ["d", "e", "f"], ["g", "h", "i"]] 
	// -> ["a", "b", "c", "d", "e", "f", "g", "h", "i"]
	.collect(Collectors.toList());

System.out.println(flatList); // [a, b, c, d, e, f, g, h, i]
```


ex) 두 개의 숫자 리스트 `[1,2,3,4,5]`과 `[6,7,8]` 가 있을때, 합이 3으로 나눠떨어지는 모든 숫자 쌍의 리스트를 반환 하시오
```java
List<Integer> numbers1 = Arrays.asList(1,2,3,4,5);  
List<Integer> numbers2 = Arrays.asList(6,7,8);  
  
  
List<List<Integer>> collect7 = numbers1.stream()  
        .flatMap(i -> numbers2.stream().map(j -> List.of(i, j)))  
        .filter(pair -> (pair.get(0) + pair.get(1)) % 3 == 0)  
        .collect(toList());  
  
System.out.println(collect7);
// [[1, 8], [2, 7], [3, 6], [4, 8], [5, 7]]
```


ex) 이름에서 사용된 모든 알파벳 중복없이 가져오기
```java
List<String> collect8 = menu.stream()  
        .map(Dish::getName)  
        .map(s -> s.split(""))  
        .flatMap(Arrays::stream)  
        .filter(s -> !s.equals(" "))  
        .distinct()  
        .collect(toList());  
  
System.out.println(collect8);  
// [p, o, r, k, b, e, f, c, h, i, n, s, a, u, t, z, w, l, m]

List<String> collect9 = menu.stream()  
        .flatMap(dish -> Arrays.stream(dish.getName().split("")))  
        .filter(s -> !s.equals(" "))  
        .distinct()  
        .collect(toList());  
  
System.out.println(collect9);
// [p, o, r, k, b, e, f, c, h, i, n, s, a, u, t, z, w, l, m]
```


## 4. 정렬
#### sorted
▶ 개념
`sorted` 메서드는 Java Stream API에서 스트림의 요소를 정렬하는 데 사용됩니다. 


▶ 어떻게 사용하는지
```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```


▶ 예시
ex) 자연 순서로 정렬  
```java
List<Integer> numbers = Arrays.asList(5, 3, 8, 1, 9, 6);  
  
List<Integer> sortedNumbers = numbers.stream()  
        .sorted() // 기본 정렬 (자연 순서)  
        .collect(Collectors.toList());  
  
System.out.println(sortedNumbers); // [1, 3, 5, 6, 8, 9]
```

ex)문자 길이 순으로, 같으면 알바펫 순으로
```java
List<String> collect = menu.stream()  
        .map(Dish::getName)  
        .sorted(Comparator.comparing(String::length)  
                .thenComparing(s -> s))  
        .collect(Collectors.toList());  
  
System.out.println(collect);
```

ex) 문자 길이의 역순으로 정렬
```java
List<String> collect1 = menu.stream()  
        .map(Dish::getName)  
        .sorted(Comparator.comparing(String::length)  
                .thenComparing(s -> s)  
                .reversed())  
        .collect(Collectors.toList());  
  
System.out.println(collect1);
```

- 🧨위의 값과 다름 : 문자열 길이로 정렬된 후, 해당 결과를 알파벳순으로 다시 역순 정렬
```
.sorted(Comparator.comparing(String::length))  
.sorted(Comparator.reverseOrder())
```
- 길이가 같을 경우 우선순위 안 정하면, reversed에서 결과값 다를 수 있음


## 5. 중간 연산 결과 확인
#### peek
▶ 개념
`peek()` 메서드는 스트림의 각 요소를 대상으로 작업을 수행할 수 있는 중간 연산입니다. 주로 디버깅 목적으로 사용되며, 스트림의 요소를 변환하거나 필터링하지 않고 단순히 요소를 살펴보는 데 사용됩니다.

▶ 어떻게 사용하는지
```java
boolean add(E element);
void add(int index, E element);
```

▶ 예시
ex) 길이로 변환
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<Integer> lengths = names.stream()  
        .peek(word -> System.out.println("단어: " + word))
        .map(String::length)
        .peek(length -> System.out.println("길이: " + length)) 
        .collect(Collectors.toList()); 
  
System.out.println(lengths);
```

```
단어: Alice
길이: 5
단어: Bob
길이: 3
단어: Charlie
길이: 7
[5, 3, 7]
```


# <코드>최종 연산
java/javaPractice/src/ModernJava/chap06
다양한 누적 요소 방식을 인수로 받아서 **스트림을 최종 결과로 도출하는 연산**을 수행해보자
## 1. 출력
#### forEach
▶ 개념
`forEach` 메서드는 스트림의 **각 요소에 대해 주어진 작업을 수행**합니다. 일반적으로 람다 표현식이나 메서드 참조를 사용하여 각 요소에 대해 수행할 작업을 지정합니다. 이 메서드는 주로 스트림의 최종 연산으로 사용되며, 스트림의 요소를 소비합니다.

▶ 어떻게 사용하는지
`forEach` 메서드는 다음과 같은 형식으로 사용됩니다:
```java
void forEach(Consumer<? super T> action);
```
- 여기서 `Consumer`는 함수형 인터페이스로, 단일 입력을 받아들이고 아무것도 반환하지 않는 연산을 나타냅니다.


▶ 예시
ex) 스트림의 각 요소를 출력  
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");  
  
names.stream().forEach(name -> System.out.printf(name));  
// names.forEach()와 같음
```

ex) menu의 각 이름 출력
```java
menu.stream()  
        .map(Dish::getName)  
        .forEach(name -> System.out.printf(name + " "));
// pork beef chicken french fries rice season fruit pizza prawns salmon 
```


## 2. 리듀싱
**Sql 처럼 모든 스트림 요소의 값을 처리해서 결과를 도출**
#### reduce ✔
▶ 개념
`reduce`는 스트림의 최종 연산으로, 스트림의 요소를 모두 소비(요소를 누적하는 방식)하고 단일 결과를 반환합니다.


▶ 어떻게 사용하는지
```java
// (1) Binary Operator를 사용한 reducing
Optional<T> reduce(BinaryOperator<T> accumulator);

// (2) 초기값과 Binary Operator를 사용한 reducing
T reduce(T identity, BinaryOperator<T> accumulator);

// (3) 매핑 함수, 초기값, Binary Operator를 사용한 reducing
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
```
- 첫 번째 인수 : 리듀싱 연산의 **시작값** 혹은 스트림에 인수가 없을 때는 반환 값
- 두 번째 인수 : stream 요소에서 **특정 타입으로의 변환하는 함수**
- 세 번째 인수 : 연산 방법- 여기서는 두 항목을 하나의 값으로 더하는 연산 수행


▶ 예시
ex) 합구하기
```java
List<Integer> numbers = Arrays.asList(4, 5, 3, 9);  
  
int numSum = numbers.stream()  
        .reduce(0, (a, b) -> a + b);  
        //.reduce(0, Integer::sum); 와 같음  
  
System.out.println(numSum); // 21
```
![[Pasted image 20221117220129.png]]


ex) 개수세기
```java
int count = menu.stream()  
        .filter(dish -> dish.getName().contains("a"))  
        .map(d -> 1)  
        .reduce(0, (a, b) -> a + b);  
        //.reduce(0, Integer::sum); 와 같음  
  
System.out.println(count); // 4
```


ex) 최대/최소 구하기
```java
int max = numbers.stream()  
        .reduce(0, (a, b) -> Integer.max(a, b));  
        //.reduce(0, Integer::max); 와 같음  
  
System.out.println(max); // 9
```
![[Pasted image 20221117220147.png]]


ex) 객체 칼로리의 합 구하기
```java
int calories = menu.stream()  
        .map(Dish::getCalories)  
        .reduce(0, Integer::sum);  
  
System.out.println(calories); // 4300
```
![[Pasted image 20221119152101.png]]


ex) 🧨 문자열로 반환
```java
// 문자열로 반환  
String menuString = menu.stream()  
        .map(Dish::getName)  
        .distinct()  
        .reduce((a, b) -> a + ", " + b).orElse("");  
        
System.out.println(menuString);  
// pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon
  
String menuString1 = menu.stream()  
        .map(Dish::getName)  
        .distinct()  
        .reduce("", (a, b) -> a + ", " + b);  
  
System.out.println(menuString1);
// , pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon
```


ex) 스트림의 각 리스트를 하나의 리스트로 병합
```java
List<List<Integer>> listOfLists = Arrays.asList(  
        Arrays.asList(1, 2, 3),  
        Arrays.asList(4, 5),  
        Arrays.asList(6, 7, 8, 9)  
);  
  
  
List<Integer> result = listOfLists.stream()  
        .reduce(new ArrayList<>(), (a, b) -> {  
            a.addAll(b);  
            return a;  
        }  
);  
System.out.println(result);  
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
List<Integer> result2 = listOfLists.stream()  
        .flatMap(List::stream)  
        .collect(Collectors.toList());  
  
System.out.println(result2);
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```


## 3. 검색
#### findAny
현재 스트림에서 **맞는 임의의 요소를 반환**
 
ex) 채식 요리를 선택하는 방법  
```java
String dish = menu.stream()  
        .filter(Dish::isVegetarian)  
        .map(Dish::getName)  
        .findAny()  
        .orElse("none");  
  
System.out.println(dish); // french fries
```

ex) 채식 요리 여부 확인
```java
boolean present = menu.stream()  
        .filter(Dish::isVegetarian)  
        .findAny()  
        .isPresent();  
  
System.out.println(present); //true
```


#### findFirst
생성된 순서가 정해진 스트림에서 **첫번째 요소를 찾기**

ex) 3으로 나누어 떨어지는 첫 번째 제곱근 값을 반환
```java
List<Integer> someNumbers = Arrays.asList(1, 2, 3, 4, 5);  
  
Integer first = someNumbers.stream()  
        .map(n -> n * n)  
        .filter(n -> n % 3 == 0)  
        .findFirst()  
        .orElse(0);  
  
System.out.println(first); // 9
```


## 4. 매칭
#### all / any / none Match ✔

ex) 적어도 한 요소와 일치하는지 확인
```java
boolean isVegetarian = menu.stream().anyMatch(Dish::isVegetarian);  
System.out.println("isVegetarian : " + isVegetarian); // true
```

ex) 모든 요소와 일치하는지 검사
```java
boolean isMatch = menu.stream().allMatch(dish -> dish.getCalories() < 1000);  
System.out.println("isMatch : " + isMatch); // true
```

ex) 모든 요소와 일치하지 않는 경우를 검사
```java
boolean isHealthy = menu.stream().noneMatch(dish -> dish.getCalories() >= 1000);  
System.out.println("isHealthy : " + isHealthy); // true
```


## 5. 통계 및 연산
#### sum ✔
ex) 합구하기
```java
Integer sum2 = menu.stream()  
        .collect(Collectors.summingInt(Dish::getCalories));  
System.out.println("Number of calories:" + sum2);  
// Number of calories:4300
  
  
int sum1 = menu.stream()  
        .mapToInt(Dish::getCalories)  
        .sum();  
System.out.println("Number of calories:" + sum1);
// Number of calories:4300
```


#### count ✔
ex) 개수세기
```java
int count2 = menu.stream()  
        .filter(dish -> dish.getName().contains("a"))  
        .collect(Collectors.counting())  
        // .count 와 같음  
        .intValue();  
  
System.out.println(count2);  // 4
  
int count1 = (int) menu.stream()  
        .filter(dish -> dish.getName().contains("a"))  
        .count();  
  
System.out.println(count1); // 4
```


#### maxBy / minBy✔
스트림의 요소를 비교하는데 사용할 **Comparator를 인수로 받음**.
ex) 최대값 구하기
```java
String maxCalories1 = menu.stream()  
        .collect(Collectors.maxBy(Comparator.comparing(Dish::getCalories)))  
        .map(Dish::getName)  
        .orElse("none");  
  
System.out.println(maxCalories1);  // pork
  
String maxCalories2 = menu.stream()  
        .max(Comparator.comparing(Dish::getCalories))  
        .map(Dish::getName)  
        .get();  
  
System.out.println(maxCalories2); // pork
```


#### summarizingInt
ex) 통계 예시
```java
IntSummaryStatistics menuStatistics = menu.stream()  
        .collect(Collectors.summarizingInt(Dish::getCalories));  
  
System.out.println(menuStatistics);  
// IntSummaryStatistics{count=9, sum=4300, min=120, average=477.777778, max=800}
System.out.println(menuStatistics.getMax());  // 800
System.out.println(menuStatistics.getAverage()); // 477.77777777777777
```
![[Pasted image 20221118181049.png]]


## 6. collect (String, Array, List)
#### String - Collectors.joining ✔
ex) 출력 문자열 연결 예시
```java
String collect = menu.stream()  
        .map(Dish::getName)  
        .collect(Collectors.joining("/"));  
System.out.println(collect);

// pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon
```


#### String - reducing ✔
▶ 개념
`reduce`는 스트림의 최종 연산으로, 스트림의 요소를 모두 소비(요소를 누적하는 방식)하고 단일 결과를 반환합니다.


▶ 어떻게 사용하는지
```java
// (1) Binary Operator를 사용한 reducing
Optional<T> reduce(BinaryOperator<T> accumulator);

// (2) 초기값과 Binary Operator를 사용한 reducing
T reduce(T identity, BinaryOperator<T> accumulator);

// (3) 매핑 함수, 초기값, Binary Operator를 사용한 reducing
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
```
- 첫 번째 인수 : 리듀싱 연산의 **시작값** 혹은 스트림에 인수가 없을 때는 반환 값
- 두 번째 인수 : stream 요소에서 **특정 타입으로의 변환하는 함수**
- 세 번째 인수 : 연산 방법- 여기서는 두 항목을 하나의 값으로 더하는 연산 수행

▶ 예시
ex) 출력 문자열 연결 예시
```java
String collect2 = menu.stream()  
        .map(Dish::getName)  
        .collect(Collectors.reducing("", (a, b) -> a + "/" + b));  
        
System.out.println(collect2);  
// /pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon


String collect3 = menu.stream()  
        .collect(Collectors.reducing("", Dish::getName, (a, b) -> a + "/" + b));  
        
System.out.println(collect3);  
// /pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon


String collect4 = menu.stream()  
        .map(Dish::getName)  
        .collect(Collectors.reducing((a, b) -> a + "/" + b))  
        .get();  
        
System.out.println(collect4);
// pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon
```


#### Array - toArray ✔
ex) `String[]` 담기
```java
String[] array = menu.stream()  
        .map(Dish::getName)  
        .toArray(String[]::new);  
  
System.out.println(Arrays.toString(array));
// [pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon]
```


ex) `Integer[]` 담기
```java
Integer[] array1 = menu.stream()  
        .map(Dish::getCalories)  
        .toArray(Integer[]::new);  
  
System.out.println(Arrays.toString(array1));
// [800, 700, 400, 530, 350, 120, 550, 400, 450]
```


ex) `Dish[]` 담기
```java
Dish[] array2 = menu.stream()  
        .toArray(Dish[]::new);  
  
System.out.println(Arrays.toString(array2));
// [pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon]
```


#### List - Collectors.toList ✔
ex) `List<String>` 에 담기
```java
List<String> collect1 = menu.stream()  
        .map(Dish::getName)  
        .collect(Collectors.toList());  
  
System.out.println(collect1);
```


ex) `List<Dish>`에 담기
```java
List<Dish> collect5 = menu.stream()  
        .collect(Collectors.toList());  
  
System.out.println(collect5);
```


## 7. Collectors.groupingBy (Map)
▶ 개념
`Collectors.groupingBy` 메서드는 스트림의 요소들을 특정 기준에 따라 그룹화하여 맵(Map)으로 반환합니다.

▶ 어떻게 사용하는지
```java
Collectors.groupingBy(classifier)
Collectors.groupingBy(classifier, downstream)
Collectors.groupingBy(classifier, supplier, downstream)
```
- `classifier`: 요소를 그룹화하는 기준이 되는 함수
- `downstream`: 그룹화된 요소들을 추가로 처리하는 Collector (예: `counting`, `summing`, `mapping` 등)
- `supplier`: 그룹을 저장할 맵의 공급자 (일반적으로 `HashMap::new` 등)


▶ 예시
#### default
ex) 재료 카테고리별 Map 담기
```java
Map<Dish.Type, List<Dish>> collect1 = menu.stream()  
        .collect(groupingBy(Dish::getType));  

System.out.println(collect1);
// {MEAT=[pork, beef, chicken], OTHER=[french fries, rice, season fruit, pizza], FISH=[prawns, salmon]}
```
![[Pasted image 20221119152916.png]]

ex) 칼로리라는 기준을 만들어서 카테고리별로 Map 담기
```java
Map<CaloricLevel, List<Dish>> collect2 = menu.stream()  
        .collect(groupingBy(dish -> {  
            if (dish.getCalories() <= 400) return CaloricLevel.DIET;  
            else if (dish.getCalories() <= 700) return CaloricLevel.NORMAL;  
            else return CaloricLevel.FAT;  
        }));  

System.out.println(collect2);
// {DIET=[chicken, rice, season fruit, prawns], FAT=[pork], NORMAL=[beef, french fries, pizza, salmon]}
```

ex) 이름의 첫글자 기준으로 Map 담기
```java
Map<String, List<Dish>> collect10 = menu.stream()  
        .collect(groupingBy(dish -> dish.getName().substring(0, 1)));  
  
System.out.println(collect10);
// {p=[pork, pizza, prawns], r=[rice], b=[beef], s=[season fruit, salmon], c=[chicken], f=[french fries]}
```

#### filtering
ex) 500칼로리 이상의 재료만 Map 타입별로 넣기
```java
Map<Dish.Type, List<Dish>> collect5 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                Collectors.filtering(dish -> dish.getCalories() > 500, toList())));  
System.out.println(collect5);
// {MEAT=[pork, beef], OTHER=[french fries, pizza], FISH=[]}

Map<Dish.Type, List<Dish>> collect3 = menu.stream()  
        .filter(dish -> dish.getCalories() > 500)  
        .collect(groupingBy(Dish::getType));  
        
System.out.println(collect3);
// {MEAT=[pork, beef], OTHER=[french fries, pizza]}
```
🧨`.filter`를 사용하면 key 값을 만들기 전에 없어짐.


#### mapping
ex) Type으로 분류한 것을 매핑하기
```java
Map<Dish.Type, List<String>> collect4 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                Collectors.mapping(Dish::getName, Collectors.toList())));  

System.out.println(collect4);  
// {MEAT=[pork, beef, chicken], OTHER=[french fries, rice, season fruit, pizza], FISH=[prawns, salmon]}


Map<Dish.Type, String> collect = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                Collectors.mapping(Dish::getName,  
                        Collectors.joining(", "))));  

System.out.println(collect);
// {MEAT=pork, beef, chicken, OTHER=french fries, rice, season fruit, pizza, FISH=prawns, salmon}
```


ex) Map의 vaule가 List인 것을 평면화하기
```java
Map<Dish.Type, Set<String>> collect6 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                flatMapping(dish -> 
	                dishTags.get(dish.getName()).stream(), toSet())));  

System.out.println(collect6);
// {MEAT=[salty, greasy, roasted, fried, crisp], OTHER=[salty, greasy, natural, light, tasty, fresh, fried], FISH=[roasted, tasty, fresh, delicious]}
```


#### 통계(counting, maxBy, summingInt)
ex) 타입별 갯수 구하기
```java
Map<Dish.Type, Long> collect7 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                Collectors.counting()));  
  
System.out.println(collect7); // {MEAT=3, OTHER=4, FISH=2}
```


ex) 타입별 최고 칼로리 구하기
```java
Map<Dish.Type, Optional<Dish>> collect8 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                Collectors.maxBy(Comparator.comparingInt(Dish::getCalories))));
  
System.out.println(collect8);
// {MEAT=Optional[pork], OTHER=Optional[pizza], FISH=Optional[salmon]}
```


ex) 타입별 합계 칼로리 구하기
```java
Map<Dish.Type, Integer> collect9 = menu.stream()  
        .collect(groupingBy(Dish::getType, Collectors.summingInt(Dish::getCalories)));  
  
System.out.println(collect9); // {MEAT=1900, OTHER=1550, FISH=850}
```


#### toSet
ex) set으로 재료 카테고리별 Map 담기
```java
Map<Dish.Type, Set<Dish>> collect12 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                toSet()));  
  
System.out.println(collect12);
// {MEAT=[beef, pork, chicken], OTHER=[rice, season fruit, french fries, pizza], FISH=[salmon, prawns]}
```


#### reducing
ex) 타입별 칼로리 합계 구하기
```java
Map<Dish.Type, Integer> collect11 = menu.stream()  
        .collect(groupingBy(Dish::getType,
	        reducing(0, Dish::getCalories, (a, b) -> a + b)));  
  
System.out.println(collect11);
// {MEAT=1900, OTHER=1550, FISH=850}
```



## 8. Collectors.toMap (Map)
▶ 개념
`toMap` 메서드는 스트림의 요소를 키와 값으로 변환하여 `Map`을 생성합니다.

▶ 어떻게 사용하는지
```java
// 기본형태
Collectors.toMap(keyMapper, valueMapper)

// 충돌 해결 전략을 포함한 형태
Collectors.toMap(keyMapper, valueMapper, mergeFunction)

// 맵 팩토리를 포함한 형태
Collectors.toMap(keyMapper, valueMapper, mapSupplier)
```
- `keyMapper`: 스트림 요소를 키로 변환하는 함수
- `valueMapper`: 스트림 요소를 값으로 변환하는 함수
- `mergeFunction`: 동일한 키가 발생했을 때 값을 병합하는 함수.
- `mapSupplier`: 결과 맵의 인스턴스를 제공하는 공급자.

▶ 예시
ex) 중복 X, `이름 : 이름길이` 생성
```java
Map<String, Integer> collect = menu.stream()  
        .collect(Collectors.toMap(  
                Dish::getName,  
                dish -> dish.getName().length()  
        ));  
  
System.out.println(collect);
// {season fruit=12, chicken=7, pizza=5, salmon=6, beef=4, pork=4, rice=4, french fries=12, prawns=6}
```

ex) 중복 X, `이름 : 칼로리` 생성
```java
Map<String, Integer> collect1 = menu.stream()  
        .collect(Collectors.toMap(  
                Dish::getName,  
                Dish::getCalories  
        ));  
  
System.out.println(collect1);
// {season fruit=120, chicken=400, pizza=550, salmon=450, beef=700, pork=800, rice=350, french fries=530, prawns=400}
```

ex) 중복 O, `Type : [이름 리스트]` 생성
```java
Map<Dish.Type, ArrayList<String>> collect2 = menu.stream()  
        .collect(Collectors.toMap(  
                Dish::getType,  
                dish -> new ArrayList<>(Collections.singletonList(dish.getName())  
                ),  
  
                (existingList, newList) -> {           
                // 병합 함수: 기존 리스트에 새 리스트 추가  
                    existingList.addAll(newList);  
                    return existingList;  
                }  
        ));  
  
System.out.println(collect2);
// {MEAT=[pork, beef, chicken], FISH=[prawns, salmon], OTHER=[french fries, rice, season fruit, pizza]}
```

ex) 중복 O, `Type : max 칼로리`
```java
Map<Dish.Type, Integer> collect3 = menu.stream()  
        .collect(Collectors.toMap(  
                Dish::getType,  
                Dish::getCalories,  
                (s1, s2) -> s1 > s2 ? s1 : s2  
        ));  
  
System.out.println(collect3);
// {MEAT=800, FISH=450, OTHER=550}
```

ex) 중복 O, `isVegetarian : [Type Set]`
```java
Map<Boolean, HashSet<Object>> collect4 = menu.stream()  
        .collect(Collectors.toMap(  
                Dish::isVegetarian,  
                dish -> {  
                    HashSet<Object> dishSet = new HashSet<>();  
                    dishSet.add(dish.getType());  
                    return dishSet;  
                },  
                (existingSet, newSet) -> {  
                    existingSet.addAll(newSet);  
                    return existingSet;  
                }  
        ));  
  
System.out.println(collect4);
// {false=[MEAT, FISH], true=[OTHER]}
```

