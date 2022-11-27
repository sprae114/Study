# SQL 기본 조회 명령어
## Select문 
#### 📌 Select문이란?
- 가져올 데이터의 열(이름)을 의미.

#### 📌 조건문과 SELECT차이는? 💥
- 조건문은 조건에 안맞는 것을 없애는 거지만, SELECT문을 활용하면 조건에 맞는경우 맞지 않는경우 + 변수를 사용해서 만드는 경우 가능

#### 📌별칭
- 테이블의 이름을 바꾸거나 간단하게 바꾸거 싶을때 사용
- 사용법 :  ” ” or AS " "

#### 📌IF/Case문
-   [IF -> 조건문을 참과거짓으로 나눌 수 있을 때 사용함 .](https://school.programmers.co.kr/learn/courses/30/lessons/59410)
![[Pasted image 20221126014033.png]]
 -   CASE -> 2가지 이상으로 나눌 수 있을 때 사용함
 ![[Pasted image 20221126014106.png]]

출처 : [[프로그래머스, SQL 문제] 중성화 여부 파악하기](https://mentha2.tistory.com/102)

#### 📌날짜 및 시간사용하기
![[Pasted image 20221126014123.png]]

- 날짜 포맷에 맞게 변경하기
![[Pasted image 20221126014145.png]]


#### 📌 수학적인 함수(MAX, MIN, length, avg, COUNT), DISTICNT, 변환
![[Pasted image 20221126014154.png]]
![[Pasted image 20221126014202.png]]
- COUNT(DISTINCT( )) : 이름의 중복되는 것을 제외한 후, 갯수를 센다

#### 📌 변수/서브 쿼리
![[Pasted image 20221126014025.png]]
💥변수 쓰고나서 ;(세미콜론) 꼭 해주자..

## FROM문
#### 📌 From문이란?
- 테이블의 출처를 의미

## WHERE문
#### 📌 WHERE문이란?
- 조건식 의미하며, 연산자사용가능

#### 📌 A is NULL/NOT NULL
- NULL 유무 알아보기
- 💥 NAME = “NULL” 과 NAME is NULL은 다른 것

#### 📌 숫자 연산자
- 연속적
- 부등호, AND, OR, [NOT] BETWEEN ~, AND ~ 등.. 사용가능
![[Pasted image 20221126014541.png]]

#### 📌 문자 연산자
- 불연속적
- AND, OR, [NOT] IN 등... 사용가능
- 💥같을때는 == 가 아닌 =, 다를때는 !=
![[Pasted image 20221126014713.png]]

#### 📌 LIKE 
- 문자열의 일부 글자로 검색
- `%` : 특정 문자 검색
    -   `우%` : 앞글자가 ‘우’이고 뒤에는 무엇이든(공백 포함) 허용  
    -   `%우` : 뒷글자가 ‘우’이고 앞에는 무엇이든(공백 포함) 허용  
    -   `%우%` : 사이에 ‘우’가 있고, 앞뒤에는 무엇이든(공백포함) 허용  
예시) [코딩테스트 연습 - 이름에 el이 들어가는 동물 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59047)

-  `_`  : 한 글자씩 매치    
    -   `_ _ 핑크`  : 블랙 핑크 처럼 글자 수는 맞춰서 무엇이든(공백 포함) 허용  
    -   `우%_%_%`

#### 📌 정규식 표현정리
-   `[ ]`  : 사이의 문자들과 매치, 하이픈사용가능 ex)a-z : a부터 z까지  
-   `a.b`  : 줄바꿈을 제외한 모든 문자와 매치ex) a0b, a=b 
-   `ca*t`  : 0번 이상 반복 매치 ex) ct,caaat  
-   `ca+t`  : 1번 이상 반복 매치 ex) cat,caaat  
-   `ca{2}t` : 2번만반복 매치 ex) caat  
-   `ca{2, 5}t`  : 2번과 5번만 반복 매치 ex) caat, caaaaat  
-   `|`  : 다자택일 ex)비누|샴푸|로션

#### 📌 정규식 사용법
-   포함되거나 제외한 경우(`[^]`) 찾기 가능  
-   처음시작(`^`)과 끝자락(`$`)이 일치하는 경우사용가능  
-   `()`괄호 안의 일치되는 부분을 묶어서 나중에 사용-> split 부분에서 나누긴 하지만 포함가능하게 나눔!!  
-   `\괄호나 \\, \n, \t, \-`는 `\`가 필요함

#### 📌 서브 쿼리
- QL의 2개의 명령문을 하나로 합친것
-   어떻게 합쳐야 할까?  
    -   일단 SELECT와 FROM은변하지 않아 영향 X  
    -   WHERE SQL문이 조건을 의미하니깐 그것을 통째로 넣어줌
- 예시) 이름이 ‘에이핑크’인 회원의키를 검색 + 그 키를 바탕으로 회원의 평균 키보다 큰 회원을 검색
![[Pasted image 20221126020115.png]]![[Pasted image 20221126020119.png]]

![[Pasted image 20221126020112.png]]

## ORDER BY 문
#### 📌 ORDER BY 문이란?
- 결과 출력되는 순서를 조절함.
- 기본 값은 ASC(오름차순)이고 DESC(내림차순) 가능
- 무엇을 기준으로 역순할건지 정해줘야함.
- 예시)[코딩테스트 연습 - 역순 정렬하기](https://school.programmers.co.kr/learn/courses/30/lessons/59035)
![[Pasted image 20221126020321.png]]

## LIMIT 문
#### 📌 LIMIT 문이란?
- 출력하는 갯수 제한
- 예시) 숫자가 한개면 0부터, 2개(a, b)면 a부터 b개만큼 개수 제한
![[Pasted image 20221126020451.png]]
![[Pasted image 20221126020454.png]]

## GROUP BY 문
#### 📌 GROUP BY 문이란?
- 그룹으로 묶어주는역할

#### 📌 왜 필요할까?
- A, B마트에 갔는데 A 마트의 각각의 제품의 금액 말고 총액을 알고 싶음.
![[Pasted image 20221126020623.png]]

#### 📌 사용함수
![[Pasted image 20221126020646.png]]

#### 📌 예시 모음
- 합계 이용
![[Pasted image 20221126020755.png]]
- 구매한 금액(가격 * 수량)
![[Pasted image 20221126020759.png]]
- [전체 회원수COUNT](https://school.programmers.co.kr/learn/courses/30/lessons/59406)
![[Pasted image 20221126020802.png]]
	[코딩테스트 연습 - 중복 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/59408)
- NULL제외하고 COUNT
![[Pasted image 20221126020808.png]]

## HAVING 문
#### 📌 HAVING 문이란?
- 집계함수의 조건문 (WHERE에서는 사용 불가) why? WHERE문이 GROUP보다앞이라서 적용 X
- 예시
![[Pasted image 20221126020903.png]]

