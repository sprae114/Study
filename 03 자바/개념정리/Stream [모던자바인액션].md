## 개념
- 스트림이란?
데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소

- 스트림의 장점
스트림을 이용하면 선언형(즉, 데이터를 처리하는 임시 구현 코드 대신 SQL처럼 질의로 표현 할 수 있다.)으로 컬렉션 데이터를 처리할 수 있다.
또한 스트림을 이용하면 멀티스레드 코드를 구현하지 않아도 데이터를 투명하게병렬로 처리할 수 있다.

## 특징
- 컬렉션의 주제는 **데이터**고, 스트림의 주제는 **계산**이다.
- 스트림은 filter, map, reduce, find, match, sort 등으로 **데이터를 조작**할 수 있다. 그리고 순차적으로 또는 병렬로 실행할 수 있다.
- 스트림은 컬렉션, 배열, I/O 자원 등의 데이터 제공 소스로부터 데이터를 소비한다. 정렬된 컬렉션으로 스트림을 생성하면 정렬이 **그대로 유지**된다.
- 트림 연산끼리 연결해서 커다란 파이프 라인을 구성할 수 있도록 스트림 자신을 반환한다. 그 덕분에 게으름(laziness), 쇼트서킷(short-cricuiting) 같은 **최적화**도 얻을 수 있다.
- 컬렉션은 반복자를 이용해서 명시적으로 반복하지만, 스트림은 **내부 반복**을 지원한다.

## 코드의 변화
- 기존코드
```java
//필터링에 맞는 결과 저장
List<Dish< lowCaloricDishes = new ArrayList<>();
for(Dish dish : menu) {
  if(dish.getCalories() < 400) {
    lowCaloricishes.add(dish);
  }
}

//정렬하기
Collections.sort(lowCaloricDishes, new Comparator<Dish>() {
  public int compare(Dish dish1, Dish dish2) {
    reutnr Integer.compare(dish1.getCalories(), dish2.getCalories());
});

//해당 칼로리의 요리 저장하기
List<Strin lowCaloricDishesName = new ArrayList<>();
for(Dish dish : lowCaloricDishes) {
  lowCaloricDishesName.add(dish.getName());
}
```

- 스트림 적용 코드
```java
List<String> lowCaloricDishesName = menu.stream()
  .filter(d -> d.getCalories() < 400) // 400 칼로리 이하의 요리 선택
  .sorted(comparing(Dish::getCalories)) // 칼로리로 요리 정렬
  .map(Dish::getName) // 요리명 추출
  .collect(toList()); // 모든 요리명을 리스트에 저장
```
![[Pasted image 20221117010655.png]]

## 스트림 연산
### 중간 연산
- 개념
중간 연산의 중요한 특징은 단말 연산을 스트림 파이프라인에 실행하기 전까지는 아무 연산도 수행하지 않는다는 것. 즉, `게으르다(lazy)`는 것이다.

- 중간 연산 종류
1.  스트림 필터링 : filter(), distinct()
2.  스트림 변환 : map(), flatMap()
3.  스트림 제한 : limit(), skip()
4.  스트림 정렬 : sorted()
5.  스트림 연산 결과 확인 : peek()

### 최종 연산
- 개념
최종 연산은 스트림 파이프라인에서 결과를 도출한다.

- 종류
1.  요소의 출력 : forEach()
2.  요소의 소모 : reduce()
3.  요소의 검색 : findFirst(), findAny()
4.  요소의 검사 : anyMatch(), allMatch(), noneMatch()
5.  요소의 통계 : count(), min(), max()
6.  요소의 연산 : sum(), average()
7.  요소의 수집 : collect()

## 스트림과 컬렉션 차이는?
스트림과 컬렉션의 가장 큰 차이는, **"데이터를 언제 계산하느냐"** 라고 합니다.

### 컬력션
-  현재 자료구조가 포함하는 모든 값을 메모리에 저장하는 자료구조(적극적 생성)
- for-each 등으로 사용자가 직접 요소를 반복해야함. (외부 반복)

### 스트림
-  이론적으로 요청할 때만 요소를 계산하는 고정된 자료구조 (게으른 생성)
-  함수에 어떤 작업을 수행할지만 지정하면 모든 것이 알아서 처리(내부 반복)


❓내부 반복의 장점은?
1) 작업을 투명하게 병렬로 처리하거나, 더 최적화된 다양한 순서로 처리
2) 데이터 표현과 하드웨어를 활용한 병렬성 구현을 자동으로 선택
![[Pasted image 20221117165158.png]]

``` java
List<String> names = menu.stream()  
    .filter(dish -> {  
      System.out.println("filtering " + dish.getName());  
      return dish.getCalories() > 300;  
    })  
    .map(dish -> {  
      System.out.println("mapping " + dish.getName());  
      return dish.getName();  
    })  
    .limit(3)  
    .collect(toList());  
System.out.println(names);
```

출력
```
filtering : pork
mapping : pork
filtering : beef
mapping : beef
filtering : chicken
mapping : chicken

[pork, beef, chicken]
```
- filter연산과 map연산이 병합되어 출력됌
- limit연산을 통해 쇼트 서킷(불필요한 연산 차단)을 함

📌 && 와 & 차이는?
-   & ,  | : 연산자의 앞 조건식과 뒤 조건식을 **둘 다** 실행 시킨다.
-   && ,  ||  : 연산자의 앞 조건식의 결과에 따라 뒤 조건식의 실행 여부를 결정

출처 : [[Java] 자바 쇼트 서킷 (short-circuit) (tistory.com)](https://junior-datalist.tistory.com/214)

## 스트림 활용
- 사용되는 배열
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

### 1. 필터링
스트림에서 **요소를 선택**하는 필터링을 제공

#### Predicate를 이용한 필터링 (Filter)
인수로 받아서 프레디케이트와 일치하는 모든 요소를 포함하는 스트림을 반환.

ex) vegetarian이 true인 것만 리스트에 넣기
```java
List<Dish> vegetarianMenu = menu.stream()  
    .filter(Dish::isVegetarian)  
    .collect(toList());  
vegetarianMenu.forEach(System.out::println);
```

출력
```
french fries
rice
season fruit
pizza
```

📌필터 표현들 모음
```java
.filter("f"::equals)
```


#### 고유 요소 필터링 (Distinct)
요소의 중복을 제거후 반환. (고유 여부는 스트림에서 만든 객체의 hashCode, equals로 결정된다.)

ex) 짝수인 것만 중복 제거해서 출력
```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4); 

numbers.stream()  
    .filter(i -> i % 2 == 0)  
    .distinct()  
    .forEach(System.out::println);
```

출력
```
2
4
```
![[Pasted image 20221117180302.png]]

### 2. 슬라이싱
스트림의 요소를 **선택**하거나 **스킵**하는 것

#### Predicate를 이용한 슬라이싱(takeWhile / dropWhile)
: 이미 정렬된 리스트/배열일 경우, 조건에 벗어날때  반복작업을 중단하는 방법
why? 큰 배열일 경우 조건에 맞지않는 그 순간 Stream 조회를 종료하는게 효과적임.

- **takeWile** 
predicate를 만족하지 않는  첫 번째 지점부터 마지막 스트림을 버림.
ex) 정렬된 menu에서 칼로리 320작은 것 찾기
```java
List<Dish> slicedMenu1
  = specialMenu.stream()
    .takeWhile(dish -> dish.getCalories() < 320)
    .collect(toList());
```

출력
```
season fruit
prawns
```


- **DROPWHILE**
첫 번째 요소부터, predicate를 처음으로 만족하는 지점까지 버리고 나머지를 반환 (takeWhile와 반대)
ex) 
```java
List<Dish> slicedMenu2
  = specialMenu.stream()
    .dropWhile(dish -> dish.getCalories() < 320)
    .collect(toList());
```

출력
```
rice
chicken
french fries
```

#### 축소 (Limit)
주어진 값 이하의 크기를 가지는 새로운 스트림을 반환

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

#### 요소 건너띄기 (Skip)
처음  n 개의 요소를 제외한(스킵한) 스트림을 반환

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

![[Pasted image 20221117183329.png]]

### 3. 스트림 매핑
특정 객체에서 **특정 데이터를 처리**하는 작업

#### 매핑 (Map)
인수로 제공된  함수는 각 요소에 적용되며 함수를 적용한 결과가 **새로운 요소로 매핑**

ex) 다양한 예시
```java
// 요리명을 추출하는 예제 
List<String> dishNames = menu.stream()  
    .map(Dish::getName)  
    .collect(toList());  
System.out.println(dishNames);  
  
// 단어 리스트의 글자수 추출 
List<String> words = Arrays.asList("Hello", "World");  
List<Integer> wordLengths = words.stream()  
    .map(String::length)  
    .collect(toList());  
System.out.println(wordLengths);  
  
// map을 연결하여 요리명의 글자수 추출
List<Integer> dishNamesLengths = menu.stream()  
        .map(Dish::getName)  
        .map(String::length)  
        .collect(toList());  
System.out.println(dishNamesLengths);  
```

출력
```
[pork, beef, chicken, french fries, rice, season fruit, pizza, prawns, salmon]

[5, 5]

[4, 4, 7, 12, 4, 12, 5, 6, 6]
```

#### 평면화 매핑 (FlatMap)

ex) [Hello, World] -> [H, e, l, o, W, r, d]로 만들기
```java
List<String> words = Arrays.asList("Hello", "World");

List<String> collect1 = words.stream()  
        .flatMap((String line) -> Arrays.stream(line.split("")))  
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

출력
```
[H, e, l, o, W, r, d]
[H, e, l, o, W, r, d]
```

![[Pasted image 20221117203513.png]]

❓if) map을 쓴다면 어떻게 결과가 나올까?
Hello와 world의 중복검사를 함.
![[Pasted image 20221117202741.png]]

ex) 🧨🧨두 개의 숫자 리스트 [1,2,3,4,5]과 [6,7,8] 가 있을때, 합이 3으로 나눠떨어지는 모든 숫자 쌍의 리스트를 반환 하시오
```java
List<Integer> numbers1 = Arrays.asList(1,2,3,4,5);  
List<Integer> numbers2 = Arrays.asList(6,7,8);  
  
List<int[]> pairs = numbers1.stream()  
    .flatMap((Integer i) -> numbers2.stream()  
        .map((Integer j) -> new int[]{i, j})  
    )  
    .filter(pair -> (pair[0] + pair[1]) % 3 == 0)  
    .collect(toList());  
    
pairs.forEach(pair -> System.out.printf("(%d, %d) ", pair[0], pair[1]));
```

출력
```
(1, 8) (2, 7) (3, 6) (4, 8) (5, 7) 
```

### 4. 검색과 매칭
#### Predicate로 매칭 확인(all / any / none Match)
ex)  다양한 매칭 예시
```java
//적어도 한 요소와 일치하는지 확인  
if(menu.stream().anyMatch(Dish::isVegetarian)) {  
    System.out.println("The menu is somewhat vegetarian friendly!!");  
}  
//모든 요소와 일치하는지 검사  
boolean isMatch = menu.stream().allMatch(dish -> dish.getCalories() < 1000);  
System.out.println(isMatch);  
  
//모든 요소와 일치하지 않는 경우를 검사  
boolean isHealthy = menu.stream().noneMatch(dish -> dish.getCalories() >= 1000);  
System.out.println(isHealthy);
```

출력
```
The menu is somewhat vegetarian friendly!!
true
true
```

#### 요소검색 (findAny / findFirst)
**findAny**
현재 스트림에서 임의의 요소를 반환
 
ex) 채식 요리를 선택하는 방법  
```java
Optional<Dish> dish = menu.stream()  
        .filter(Dish::isVegetarian)  
        .findAny();//findAny는 Optional 객체를 반환  
System.out.println(dish);
```

출력
```
Optional[french fries]
```


**findFirst**
생성된 순서가 정해진 스트림에서 첫번째 요소를 찾기

ex) 3으로 나누어 떨어지는 첫 번째 제곱근 값을 반환
```java
List<Integer> someNumbers = Arrays.asList(1, 2, 3, 4, 5);  
Optional<Integer> first = someNumbers.stream()  
        .map(n -> n * n)  
        .filter(n -> n % 3 == 0)
        .findFirst();  
System.out.println(first);
```

출력
```
Optional[9]
```

### 5. 리듀싱
Sql 처럼 모든 스트림 요소의 값을 처리해서 결과를 도출

#### 연산(reduce / Sum)
ex) 합구하기
```java
List<Integer> numbers = Arrays.asList(3, 4, 5, 1, 2);  

int sum = numbers.stream()  
        .reduce(0, (a, b) -> a + b);  
System.out.println(sum);  


int sum2 = numbers.stream()  
        .reduce(0, Integer::sum);  
System.out.println(sum2);  


int sum3 = numbers.stream()  
        .mapToInt(n -> n)  
        .sum();  
System.out.println(sum3);
```

출력
```
15
15
15
```
![[Pasted image 20221117220129.png]]

ex) 객체일때
```java
int calories = menu.stream()  
    .map(Dish::getCalories)  
    .reduce(0, Integer::sum);  
System.out.println("Number of calories:" + calories);


int calories2 = menu.stream()  
    .mapToInt(Dish::getCalories)  
    .sum();
System.out.println("Number of calories:" + calories2);
```

출력
```
Number of calories:4300
Number of calories:4300
```

ex) 갯수세기
```java
int count = menu.stream()  
        .map(d -> 1)  
        .reduce(0, (a,b) -> a + b);  
System.out.println(count);  


long count1 = menu.stream()  
        .mapToInt(n -> 1)  
        .count();  
System.out.println(count1);
```

출력
```
9
9
```


ex) 문자열로 반환
```java
String menuString = menu.stream()  
        .map(Dish::getName)  
        .distinct()  
        .reduce("", (a, b) -> a + b);  
System.out.println(menuString);
```

출력
```
porkbeefchickenfrench friesriceseason fruitpizzaprawnssalmon
```




#### 최대/최소 구하기(Max / Min)
ex) 최대/최소 구하기
```java
List<Integer> numbers = Arrays.asList(3, 4, 5, 1, 2);  

int max = numbers.stream().reduce(0, (a, b) -> Integer.max(a, b));  
System.out.println(max);  
  
Optional<Integer> min = numbers.stream().reduce(Integer::min);  
min.ifPresent(System.out::println);
```

출력
```
5
1
```

![[Pasted image 20221117220147.png]]

### 6 숫자형 스트림
reduce로 하면 코드가 좀더 복잡하짐 + Integer에서 int로 전환하는 박싱비용 소모된다. 그래서 기본형에 특화된 스트림을 제공함.


#### 기본형 특화 스트림 (mapToInt)
``` java
//기존코드
int calories = menu.stream()
  .map(Dish::getCalories)
  .reduce(0, Integer::sum);

//바뀐코드
int calories = menu.stream()
  .mapToInt(Dish::getCalories)
  .sum();
```

출력
```
4300
4300
```

#### 객체 스트림으로 복원하기 (mapToInt / boxed)
```java
// 스트림을 숫자 스트림으로 변환
IntStream intStream = menu.stream().mapToInt(Dish::getCalories); 

// 숫자 스트림을 스트림으로 변환
Stream<Integer> stream = intStream.boxed(); 
```

#### 숫자 범위 (rangeClosed)
ex) 1 ~ 100까지 짝수 개수세기
```java
// 숫자 범위
IntStream evenNumbers = IntStream.rangeClosed(1, 100)  
    .filter(n -> n % 2 == 0);  
System.out.println(evenNumbers.count());
```

출력
```
50
```


ex) x = 1 ~ 10, y = 1 ~ 100까지로 만들 수 있는 피타고라스 수는?
```java
Stream<int[]> n = IntStream.rangeClosed(1, 10).boxed()  
    .flatMap(a -> IntStream.rangeClosed(a, 100)  
        .filter(b -> Math.sqrt(a * a + b * b) % 1 == 0).boxed()  
        .map(b -> new int[] { a, b, (int) Math.sqrt(a * a + b * b) }));  
n.forEach(t -> System.out.println(t[0] + ", " + t[1] + ", " + t[2]));  
System.out.println();
```

출력
```
3, 4, 5
5, 12, 13
6, 8, 10
7, 24, 25
8, 15, 17
9, 12, 15
9, 40, 41
10, 24, 26
```

### 7. 스트림 만들기
#### 값으로 스트림 만들기 (Stream.of)
임의의 수를 인수로 받는 정적 메서드 `Stream.of`를 이용해서 스트림을 만들 수 있음.

ex) 문자열 스트림을 대문자로 바꿔서 출력하는 예제
```java
Stream<String> stream = Stream.of("Java 8", "Lambdas", "In", "Action");  
stream.map(String::toUpperCase)
	  .forEach(System.out::println);
```

출력
```
JAVA 8
LAMBDAS
IN
ACTION
```

#### 배열로 스트림 만들기 (Array.stream)

ex) 배열스트림을 활용해서 합 구하기
```java
int[] numbers = {2, 3, 5, 7, 11, 13};
int sum = Arrays.stream(numbers).sum();
```

출력
```
41
```


#### 함수로 무한 스트림 만들기1 (Iterate)
ex) 0부터 2씩 더해서 5개 출력하기
```java
Stream.iterate(0, n -> n + 2)  
    .limit(5)  
    .forEach(System.out::println);
```

출력
```
0
2
4
6
8
```

ex) 피보나치 수열 만들기
```java
Stream.iterate(new int[] { 0, 1 }, t -> new int[] { t[1], t[0] + t[1] })  
        .limit(10)  
        .forEach(t -> System.out.printf("%d ", t[0]));  
```

출력
```
1 1 2 3 5 8 13 21 34 55 
```

ex) 조건을 이용해서 종료하는 방법
```java
List<Integer> collect1 = Stream.iterate(0, n -> n < 20, n -> n + 3)  
        .collect(Collectors.toList());  
System.out.println(collect1);  


List<Integer> collect2 = Stream.iterate(0,  n -> n + 3)  
        .takeWhile(n -> n < 20)  
        .collect(Collectors.toList());  
System.out.println(collect2);  
```

출력
```
[0, 3, 6, 9, 12, 15, 18]

[0, 3, 6, 9, 12, 15, 18]
```

#### 함수로 무한 스트림 만들기2 (Generate)

ex) 임의의 double 스트림
```java
List<Double> collect = Stream.generate(Math::random)  
        .limit(3)  
        .collect(Collectors.toList());  
System.out.println(collect);
```

출력
```
[0.4383143801392815, 0.7571514993934333, 0.14408242014275896]
```

📌 난수만들기 더 궁금하면 [스트림 데이터 소스 별 생성 방법 정리 ](https://velog.io/@max9106/Java-%EC%8A%A4%ED%8A%B8%EB%A6%BCstream-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%86%8C%EC%8A%A4-%EB%B3%84-%EC%83%9D%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC)


ex) 요소 1을 갖는 스트림  
```java
IntStream.generate(() -> 1)  
    .limit(3)  
    .forEach(System.out::println);
```

출력
```
1
1
1
```


## 스트림으로 데이터 수집
최종연산인 **reduce** 처럼 다양한 누적 요소 방식을 인수로 받아서 스트림을 최종 결과로 도출하는 리듀싱 연산을 수행해보자

###  리듀싱과 요약
####  collect으로 개수세기 (count)
why count()가 있는데 이걸 쓰는거지? collector와 섞어쓸때 용의함

ex) 개수세기
```java
long menuCount = menu.stream().collect(Collectors.counting());
//long menuCount = menu.stream().count(); 와 같은 결과
```

출력
```
9
```

#### 최대/최소값 구하기 (maxBy / minBy)
스트림의 요소를 비교하는데 사용할 Comparator를 인수로 받음.
ex) 최대값 구하기
```java
Comparator<Dish> dishCaloriesComparator = Comparator.comparingInt(Dish::getCalories);  

Optional<Dish> mostCalorieDish = menu.stream().collect(Collectors.maxBy(dishCaloriesComparator));  
System.out.println(mostCalorieDish.get());
```

출력
```
pork
```

#### 요약연산 (summarizingInt / summingInt)

ex) 통계 예시
```java
int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));  
System.out.println(totalCalories);  
  
Double average = menu.stream().collect(averagingInt(Dish::getCalories));  
System.out.println(average);  
  
IntSummaryStatistics menuStatistics = menu.stream().collect(summarizingInt(Dish::getCalories));  
System.out.println(menuStatistics);  
System.out.println(menuStatistics.getMax());
```

출력
```
4300

477.77777

IntSummaryStatistics{count=9, sum=4300, min=120, average=477.777778, max=800}
800
```

![[Pasted image 20221118181049.png]]

#### 문자열 연결 ( joining )
ex) 출력 문자열 연결 예시
```java
String collect = menu.stream()  
        .map(Dish::getName)  
        .collect(joining("/"));  
System.out.println(collect);
```

출력
```
pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon
```

#### 범용 리듀싱 요약(reducing)
첫 번째 인수 : 리듀싱 연산의 시작값 혹은 스트림에 인수가 없을 때는 반환 값
두 번째 인수 : stream 요소에서 특정 타입으로의 변환하는 함수
세 번째 인수 : 연산 방법- 여기서는 두 항목을 하나의 값으로 더하는 연산 수행

ex) 리듀싱 연산
```java
Integer sumCalories = menu.stream()  
        .collect(reducing(0, Dish::getCalories, (i, j) -> i + j));  
System.out.println(sumCalories);  
  
Integer sumCalories2 = menu.stream()  
        .collect(reducing(0, Dish::getCalories, Integer::sum));  
System.out.println(sumCalories2);  
  
String collect3 = menu.stream()  
        .collect(reducing("", Dish::getName, (a, b) -> a + "/" + b));  
System.out.println(collect3);

String collect4 = menu.stream()  
        .map(Dish::getName)  
        .collect(reducing((a, b) -> a + "/" + b))  
        .get();  
System.out.println(collect4);
```

출력
```
4300
4300

/pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon
pork/beef/chicken/french fries/rice/season fruit/pizza/prawns/salmon
```
![[Pasted image 20221119152101.png]]

❓ collect vs reduce의 차이는?
-   collect : 도출하려는 결과를 누적하는 컨테이너를 바꾸도록 설계된 메서드
-   reduce : 두 값을 하나로 도출하는 불변형 연산이라는 점에서  두 메서드의 의미가 다릅니다.

### 그룹화
#### 그룹 묶기 (groupingBy)
ex) 타입별로 묶는 예시
```java
Map<Dish.Type, List<Dish>> collect1 = menu.stream()  
        .collect(groupingBy(Dish::getType));  
System.out.println(collect1);
```

출력
```
{OTHER=[french fries, rice, season fruit, pizza], MEAT=[pork, beef, chicken], FISH=[prawns, salmon]}
```

![[Pasted image 20221119152916.png]]

ex) 분류함수 만들어서 반환 예시
```java
public enum CaloricLevel { DIET, NORMAL, FAT };

Map<CaloricLevel, List<Dish>> collect2 = menu.stream()  
        .collect(groupingBy(dish -> {  
            if (dish.getCalories() <= 400) return CaloricLevel.DIET;  
            else if (dish.getCalories() <= 700) return CaloricLevel.NORMAL;  
            else return CaloricLevel.FAT;  
        }));  
System.out.println(collect2);
```

출력
```
{DIET=[chicken, rice, season fruit, prawns], NORMAL=[beef, french fries, pizza, salmon], FAT=[pork]}
```


#### 그룹화된 요소 조작 (filtering / mapping)
ex) 각 결과 그룹 요소를 조작하는 연산 예시
```java
//500칼로리 이상의 재료만 Map 타입별로 넣기  
Map<Dish.Type, List<Dish>> collect3 = menu.stream()  
        .filter(dish -> dish.getCalories() > 500)  
        .collect(groupingBy(Dish::getType));  
System.out.println(collect3);  
  
//500칼로리 이상의 재료만 Map 타입별로 넣기(없는 타입도 표시하기)  
Map<Dish.Type, List<Dish>> collect5 = menu.stream().collect(  
        groupingBy(Dish::getType,  
                filtering(dish -> dish.getCalories() > 500, toList())));  
System.out.println(collect5);  
  
//각 재료를 Map에 타입별로 넣기  
Map<Dish.Type, List<String>> collect4 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                mapping(Dish::getName, toList())));  
System.out.println(collect4);  
  
//Map의 vaule가 List인 것을 평면화하기  
Map<Dish.Type, Set<String>> collect6 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                flatMapping(dish -> dishTags.get(dish.getName()).stream(), toSet())));  
System.out.println(collect6);
```

출력
```
{MEAT=[pork, beef], OTHER=[french fries, pizza]}

{MEAT=[pork, beef], FISH=[], OTHER=[french fries, pizza]}

{MEAT=[pork, beef, chicken], FISH=[prawns, salmon], OTHER=[french fries, rice, season fruit, pizza]}

{MEAT=[greasy, salty, salty, roasted, fried, crisp], FISH=[tasty, roasted, delicious, fresh], OTHER=[greasy, fried, light, natural, fresh, natural, tasty, salty]}
```

📌다수준의 그룹화는 필요할때 찾아보자(P.213)

ex) 인수를 다른 것으 넘겨주는 경우 예시
```java
//counting()을 인수로 넘겨주는 경우  
Map<Dish.Type, Long> collect7 = menu.stream()  
        .collect(groupingBy(Dish::getType, counting()));  
System.out.println(collect7);  
  
//maxby를 인수로 넘겨주는 경우  
Map<Dish.Type, Optional<Dish>> collect8 = menu.stream()  
        .collect(groupingBy(Dish::getType,  
                maxBy(Comparator.comparingInt(Dish::getCalories))));  
System.out.println(collect8);
```

출력
```
{MEAT=3, FISH=2, OTHER=4}
{MEAT=Optional[pork], FISH=Optional[salmon], OTHER=Optional[pizza]}
```

#### 분할
분할은 무조건 2개의 그룹으로 그룹화할 수 있는 것(Boolean을 반환하므로 맵의 키 형식은 Boolean)

ex) 분할 예시
```java
//채식 유무로 나누기  
Map<Boolean, List<Dish>> collect = menu.stream()  
        .collect(partitioningBy(Dish::isVegetarian));  
System.out.println(collect);  
System.out.println(collect.get(true));  
  
//채식 유무로 나누는데 group된거 넣기  
Map<Boolean, Map<Dish.Type, List<Dish>>> collect1 = menu.stream()  
        .collect(partitioningBy(Dish::isVegetarian, groupingBy(Dish::getType)));  
System.out.println(collect1);  
  
//채식 요리와 아닌 것중에서 가장 높은 칼로리 음식 찾기  
Map<Boolean, Dish> collect2 = menu.stream().collect(  
        partitioningBy(Dish::isVegetarian,  
                collectingAndThen(  
                        maxBy(comparingInt(Dish::getCalories)), Optional::get)));  
System.out.println(collect2);  
  
//채식 요리와 아닌 것 개수세기  
Map<Boolean, Long> collect3 = menu.stream().collect(  
        partitioningBy(Dish::isVegetarian, counting()));  
System.out.println(collect3);
```

출력
```
{false=[pork, beef, chicken, prawns, salmon], true=[french fries, rice, season fruit, pizza]}
[french fries, rice, season fruit, pizza]

{false={MEAT=[pork, beef, chicken], FISH=[prawns, salmon]}, true={OTHER=[french fries, rice, season fruit, pizza]}}

{false=pork, true=pizza}

{false=5, true=4}
```


ex) 소수 판별하기
```java
System.out.println(partitionPrimes(20));  
  
public static Map<Boolean, List<Integer>> partitionPrimes(int n) {  
  return IntStream.rangeClosed(2, n).boxed()  
          .collect(partitioningBy(candidate -> isPrime(candidate)));  
}  
  
public static boolean isPrime(int candidate) {  
  return IntStream.rangeClosed(2, candidate - 1)  
          .limit((long) Math.floor(Math.sqrt(candidate)) - 1)  
          .noneMatch(i -> candidate % i == 0);  
}
```

출력
```
{false=[4, 6, 8, 9, 10, 12, 14, 15, 16, 18, 20], true=[2, 3, 5, 7, 11, 13, 17, 19]}
```



## 기타
### Optional 처리하는 방법(isPresent / get / orElse)
1) isPresent()
값을 포함하면 true, 값을 포함하지 않으면 false
```java
boolean present = menu.stream()  
        .filter(Dish::isVegetarian)  
        .findAny()//findAny는 Optional 객체를 반환  
        .isPresent();  
System.out.println(present);
```

2) ifPresent()
값을 가지고 있으면 실행 값이 없으면 넘어감
```java
Optional<Integer> min = numbers.stream().reduce(Integer::min);  
min.ifPresent(System.out::println);
```

3) T get()
값이 존재하면 반환하고, 아니면 NoSuchElementException

4) T orElse()
값이 존재하면 반환하고, 아니면 기본값을 반환함.

