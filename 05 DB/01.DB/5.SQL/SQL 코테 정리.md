#복습SQL

---
# SQL 기본 조회 명령어
연습할 수 있는 사이트 [MySQL Tryit Editor v1.0 (w3schools.com)](https://www.w3schools.com/mysql/trymysql.asp?filename=trysql_select_all)

## Select문 ✔
#### Select문이란?
- **가져올 데이터**의 열(이름)을 의미.


#### WHERE과 SELECT문의 IF의 차이는?
- **WHERE문**은 개별 행의 조건을 필터링하는 데 사용함. 즉, select 값의 **변화 줄 수 X**
- **SELECT문의 IF**는 열 그대로가 아닌 열의 범위로 할당해주는 것. 즉, select 값의 **변화 줄 수 O**

예시 1) employees 테이블에서 급여가 5000 이상인 직원의 이름과 급여를 선택하는 쿼리입니다.
```sql
SELECT name, salary
FROM employees
WHERE salary >= 5000;
```

예시 2) products 테이블에서 가격이 10에서 50 사이이고 재고량이 0보다 큰 상품들을 선택하는 쿼리입니다.
```sql
SELECT *
FROM products
WHERE price BETWEEN 10 AND 50 AND inventory > 0;
```

예시 3)  products테이블에서 이름과 가격에 따라 가격 범주를 계산하여 선택하는 쿼리
```sql
SELECT product_name,
   CASE
	   WHEN price > 100 THEN 'Expensive'
	   WHEN price > 50 THEN 'Moderate'
	   ELSE 'Affordable'
   END AS price_category
FROM products;
```

위의 쿼리는 products 테이블에서 상품의 **이름과 가격에 따라 가격 범주(price_category)를 지정하여 선택**합니다. 가격이 100보다 크면 'Expensive', 50보다 크면 'Moderate', 그렇지 않으면 'Affordable'로 지정합니다.


#### 별칭
- 테이블의 **이름을 바꾸거**나 간단하게 바꾸고 싶을때 사용
- 사용법 :  ” ” or AS " "

#### 문자열 연산
1. CONCAT 함수
- 두 개 이상의 **문자열을 연결**합니다.
- 칼럼명과 같은 변수 사용가능
```sql
SELECT CONCAT('Hello', ' ', 'World');    //Hello World
```

2. SUBSTRING 함수
- 문자열의 일부분을 추출합니다. 
- SUBSTRING("문자열", "시작위치", "길이") : 지정한 위치에서 지정한 문자열 길이만큼 자를 때 사용
```sql
SELECT SUBSTRING('Hello World', 7, 5);     //World
```

3. REPLACE 함수
- 문자열에서 특정 문자 또는 문자열을 다른 문자 또는 문자열로 대체합니다.
```sql
SELECT REPLACE('Hello World', 'World', 'Universe');    //Hello Universe
```

4. UPPER/LOWER 함수
- 문자열을 대문자 또는 소문자로 변환합니다.
```sql
SELECT UPPER('hello');     //HELLO
SELECT LOWER('WORLD');     //world
```



#### IF/Case문
출처 : [[프로그래머스, SQL 문제] 중성화 여부 파악하기](https://mentha2.tistory.com/102)
▶  [IF -> 조건문을 참과거짓으로 나눌 수 있을 때 사용함 .](https://school.programmers.co.kr/learn/courses/30/lessons/59410)
```
IF (조건문, 참일때 값, 거짓일때 값)  
```
- seq 값에 따라 IF 문을 사용하여 결과를 선택하는 쿼리
```sql
SELECT  A.seq, IF(A.seq <= 3, 'A', 'B') AS if_result 
FROM Table A
```
- null 값 표현하는 IF문
```sql
SELECT ANIMAL_TYPE, IF(NAME IS NULL, 'No name', NAME) AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

▶ CASE -> 2가지 이상으로 나눌 수 있을 때 사용함
```
﻿CASE
	﻿WHEN 조건1 THEN '조건1 반환값' 
	﻿WHEN 조건2 THEN '조건2 반환값' 
	﻿ELSE '충족되는 조건 없을때 반환값'
END
```
- seq 값에 따라 다른 값을 반환하는 CASE 문을 사용하여 결과를 선택하는 쿼리
```sql
SELECT
	seq,
	CASE
		WHEN (u.seq BETWEEN 1 AND 3) THEN 'A' 
		WHEN (u.seq BETWEEN 4 AND 6) THEN 'B' 
		ELSE 'C'
	END AS case_result
FROM Table u
```

1.  `SELECT seq, CASE WHEN (u.seq BETWEEN 1 AND 3) THEN 'A' WHEN (u.seq BETWEEN 4 AND 6) THEN 'B' ELSE 'C' END AS case_result`
- seq와 case_result 열을 선택합니다.
- CASE 문을 사용하여 seq의 값에 따라서 결과를 선택합니다.
- seq 값이 1과 3 사이에 있으면 'A'를 반환하고, 4와 6 사이에 있으면 'B'를 반환하고, 그 외의 경우에는 'C'를 반환합니다.
- case_result 열에 해당 결과를 할당합니다.


#### 날짜 및 시간사용하기
- 날짜 관련 텍스트 추출하기(**날짜 열이 있을때**)
```sql
SELECT DATETIME as '전체시간' FROM ANIMAL_INS;     # 2018-01-22 14:32:00
SELECT year (DATETIME) as '몇 년'FROM ANIMAL_INS LIMIT 1;    # 2018
SELECT month(DATETIME) as '몇 월'FROM ANIMAL_INS LIMIT 1;    # 1
SELECT day(DATETIME) as '몇 일' FROM ANIMAL_INS LIMIT 1;     # 22
SELECT hour (DATETIME) as '몇 시간' FROM ANIMAL_INS;         # 14
SELECT minute(DATETIME) as '몇 분'FROM ANIMAL_INS LIMIT 1;   # 32
SELECT second (DATETIME) as '몇 초'FROM ANIMAL_INS LIMIT 1;  # 0
```

- 날짜 포맷에 맞게 변경하기
```sql
SELECT DATE_FORMAT(DATETIME, "20%Y-%m-%d") AS 날짜 
FROM ANIMAL_INS
```

```
서식별 표현하는 법 예시
%Y : 1999, %y : 99
%M : July, %m : 07
%D : 4th,  %d : 04
```


#### 수학적인 함수(MAX, MIN, length, avg, COUNT), DISTICNT, 변환
1. MAX 함수 예시 (employees 테이블에서 **가장 높은 급여를 찾아내는 쿼리**)
```sql
SELECT MAX(salary) AS max_salary
FROM employees;
```

2. MIN 함수 예시 (products 테이블에서 **가장 낮은 가격을 찾아내는 쿼리**)
```sql
SELECT MIN(price) AS min_price
FROM products;
```

3. LENGTH 함수 예시 (products 테이블에서 각 상품의 이름과 **이름의 길이를 반환하는 쿼리**)
```sql
SELECT product_name, LENGTH(product_name) AS name_length
FROM products;
```

4. AVG 함수 예시 (orders 테이블에서 수량의 **평균값을 계산하는 쿼리**)
```sql
SELECT AVG(quantity) AS avg_quantity
FROM orders;
```

5. COUNT 함수 예시(customers 테이블에서 **전체 고객의 수를 계산하는 쿼리**)
```sql
SELECT COUNT(*) AS total_customers
FROM customers;
```

6. DISTINCT 예시( Products 테이블에서 **중복을 제외한 고유한 CategoryID 값을 선택**)
```sql
SELECT DISTINCT CategoryID
FROM Products;
```

7. 실습해보며 응용하기
```sql
SELECT CategoryID AS '카테고리ID',
       SUM(Price) / COUNT(Price) AS '평균가격',
       CONVERT(AVG(Price), CHAR) AS '평균가격_문자열',
       ROUND(AVG(Price), 1) AS '평균가격_반올림',
       COUNT(Price) AS '상품수',
       COUNT(DISTINCT(Price)) AS '고유가격수'
FROM Products
GROUP BY CategoryID;
```
- COUNT(DISTINCT( )) : 이름의 **중복되는 것을 제외**한 후, 갯수를 센다


#### 변수/서브 쿼리
예시) ANIMAL_OUTS 테이블에서 **시간별 동물이 나간 횟수를 계산하는 쿼리**입니다.
[코딩테스트 연습 - 입양 시각 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/59413)
```sql
﻿SET @HOUR := -1;
SELECT (@HOUR := @HOUR +1) AS HOUR,
	(SELECT COUNT(HOUR(DATETIME)) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @HOUR) AS 'COUNT'
FROM ANIMAL_OUTS
WHERE @HOUR < 23;
```
위의 쿼리를 실행하면 **0부터 22까지의 시간대별로 ANIMAL_OUTS 테이블에서 해당 시간대의 동물이 나간 횟수가 반환**됩니다. 쿼리를 분석하면 

1. `SET @HOUR := -1`;
   - @HOUR 변수를 -1로 초기화합니다.
2. `SELECT (@HOUR := @HOUR + 1) AS HOUR, (SELECT COUNT(HOUR(DATETIME)) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @HOUR) AS 'COUNT'`
   - @HOUR 변수의 값을 1씩 증가시키면서 HOUR 열로 반환합니다.
   - 서브쿼리를 사용하여 ANIMAL_OUTS 테이블에서 HOUR(DATETIME) 값이 @HOUR과 일치하는 행의 개수를 계산합니다.
   - 결과를 'COUNT' 별칭으로 반환합니다.
3. `FROM ANIMAL_OUTS`
   - ANIMAL_OUTS 테이블에서 데이터를 선택합니다.
4. `WHERE @HOUR < 23`;
   - @HOUR 값이 23보다 작은 동안에만 쿼리를 실행합니다.

💥변수 쓰고나서 ;(세미콜론) 꼭 해주자..


## FROM문
#### From문이란?
- **테이블의 출처**를 의미

## WHERE문
#### WHERE문이란?
- **조건식 의미**하며, 연산자사용가능


#### A is NULL/NOT NULL
- NULL 유무 알아보기
- 💥 **NAME = “NULL” 은 문자 NULL이 있나 확인하는 것이고 NAME IS NULL은 테이블이 빈칸인지 알아보는 함수**


#### 숫자 연산자
- 연속적
- 부등호, AND, OR, [NOT] BETWEEN ~, AND ~ 등.. 사용가능
- 예시) `WHERE height >= 163 AND height <= 165;`


#### 문자 연산자
- 불연속적
- AND, OR, [NOT] IN 등... 사용가능
- 💥**같을때는 == 가 아닌 =, 다를때는 !=**
- 예시) `WHERE addr IN('경기', '전남', '경남')`


#### LIKE 
- 문자열의 일부 글자로 검색
- `%` : 특정 문자 검색
```
우% : 앞글자가 ‘우’이고 뒤에는 무엇이든(공백 포함) 허용  
%우 : 뒷글자가 ‘우’이고 앞에는 무엇이든(공백 포함) 허용  
%우% : 사이에 ‘우’가 있고, 앞뒤에는 무엇이든(공백포함) 허용  
```

예시 1) customers 테이블에서 이름이 **'John'으로 시작하는 고객들**을 선택하는 쿼리입니다.
```sql
SELECT *
FROM customers
WHERE name LIKE 'John%';
```

예시 2) products 테이블에서 상품명에 **'apple'이 포함된 상품들**을 선택하는 쿼리입니다.
```sql
SELECT *
FROM products
WHERE product_name LIKE '%apple%';
```

풀어보기) [코딩테스트 연습 - 이름에 el이 들어가는 동물 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59047)

- `_`  : 한 글자씩 매치    
    - `_ _ 핑크`  : 블랙 핑크 처럼 글자 수는 맞춰서 무엇이든(공백 포함) 허용  
    - `우%_%_%`

예시) customers 테이블에서 **이름이 5글자이고 두 번째 글자가 'o'인 고객들을 선택**하는 쿼리입니다.
```sql
SELECT *
FROM customers
WHERE name LIKE '_o___';
```


#### 정규식 표현정리
- `[ ]`  : 사이의 문자들과 매치, 하이픈사용가능 ex)a-z : a부터 z까지  
- `a.b`  : 줄바꿈을 제외한 모든 문자와 매치ex) a0b, a=b 
- `ca*t`  : 0번 이상 반복 매치 ex) ct,caaat  
- `ca+t`  : 1번 이상 반복 매치 ex) cat,caaat  
- `ca{2}t` : 2번만반복 매치 ex) caat  
- `ca{2, 5}t`  : 2번과 5번만 반복 매치 ex) caat, caaaaat  
- `|`  : 다자택일 ex)비누|샴푸|로션
- `^` :  패턴의 시작을 의미
- `$` : 패턴의 끝을 의미


#### 정규식 사용법
- 포함되거나 제외한 경우(`[^]`) 찾기 가능  
- `()`괄호 안의 일치되는 부분을 묶어서 나중에 사용-> split 부분에서 나누긴 하지만 포함가능하게 나눔!!  
- `\괄호나 \\, \n, \t, \-`는 `\`가 필요함

예시 1) products 테이블에서 상품명이 **'Apple' 또는 'Banana'로 시작하는 상품들을 선택**하는 쿼리입니다.
```sql
SELECT *
FROM products
WHERE product_name REGEXP '^(Apple|Banana)';
```

예시 2) customers 테이블에서 **이름이 'J' 또는 'K'로 시작**하고, **이메일 주소가 '@gmail.com' 도메인**을 갖는 고객들을 선택하는 쿼리입니다.
```sql
SELECT *
FROM customers
WHERE name REGEXP '^[JK].*' AND email REGEXP '@gmail\.com$';
```
- `email REGEXP '@gmail\.com$'`는 email 열에서 `@gmail.com`과 정확히 일치하는 이메일 주소를 선택하는 조건을 나타냅니다.


#### 서브 쿼리
- **SQL의 2개의 명령문을 하나로 합친것**
- 어떻게 합쳐야 할까?  
    - 일단 SELECT와 FROM은변하지 않아 영향 X  
    - WHERE SQL문이 조건을 의미하니깐 그것을 통째로 넣어줌

예시) 이름이 ‘에이핑크’인 회원의키를 검색 + 그 키를 바탕으로 회원의 평균 키보다 큰 회원을 검색
```sql
# 합치고 싶은 두개의 SQL
SELECT height FROM member WHERE mem_name = '에이핑크'
SELECT mem_name, height FROM member  WHERE height > 164;

# 서브 쿼리로 합친 SQL
SELECT mem_name, height 
FROM member 
WHERE height > 164 
AND mem_name = (SELECT height FROM member WHERE mem_name = '에이핑크');

# join을 사용한 경우
SELECT m1.mem_name, m1.height 
FROM member m1
JOIN member m2 ON m1.height = m2.height
WHERE m2.mem_name = '에이핑크' AND m1.height > 164;
```

- 서로 다른 테이블에서도 서브쿼리가 가능할까?
예시) 각 부서의 평균 급여보다 많은 급여를 받는 직원들을 조회하려면 쿼리입니다.
```sql
SELECT e.employee_id, e.first_name, e.last_name, e.salary, e.department_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```


## ORDER BY 문
#### ORDER BY 문이란?
- **결과 출력되는 순서를 조절**함.
- 기본 값은 ASC(오름차순)이고 DESC(내림차순) 가능
- 무엇을 기준으로 역순할건지 정해줘야함.

예시)[코딩테스트 연습 - 역순 정렬하기](https://school.programmers.co.kr/learn/courses/30/lessons/59035)
```sql
ORDER BY height DESC, debut_date ASC;
```


## LIMIT 문
#### LIMIT 문이란?
- **출력 갯수 제한**

예시1) customers 테이블에서 상위 5개의 고객을 선택하는 쿼리입니다.
```sql
SELECT * FROM customers LIMIT 5;
```

예시2) employees 테이블에서 10번째부터 20번째까지의 직원들을 선택하는 쿼리입니다.
```sql
SELECT * FROM employees LIMIT 10, 10;
```


## GROUP BY문
#### GROUP BY 문이란?
=> 데이터를 그룹화하고 그룹 단위로 집계 함수를 사용하여 **데이터를 요약하는 데 사용함.** **"~별로 계산**"하는 말이 있으면 사용하자.

#### 왜 필요할까?
- A, B마트에 갔는데 A 마트의 **각각의 제품의 금액 말고 총액**을 알고 싶음.
![[Pasted image 20221126020623.png]]

#### 사용함수
![[Pasted image 20221126020646.png]]

#### GROUP BY문 구조
```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table_name
WHERE conditions
GROUP BY column1, column2, ...
```
여기서 **column1, column2 등은 그룹화할 열**을 나타내며, **aggregate_function은 그룹 내에서 계산할 집계 함수**를 나타냅니다. 집계 함수는 SUM, COUNT, AVG, MIN, MAX 등이 있습니다. table은 데이터를 가져올 테이블 이름이고, **conditions은 필요에 따라 추가적인 조건을 지정할 수 있는 WHERE 절**입니다.


#### GROUP BY 예시
예시1) orders 테이블에서 **고객별로 주문된 총 금액을 계산**하는 쿼리입니다.
```sql
SELECT customer_id, SUM(order_amount) AS total_amount
FROM orders
GROUP BY customer_id;
```

예시 2) employees 테이블에서 **부서별로 직원 수를 계산**하는 쿼리입니다.
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

예시 3) products 테이블에서 **카테고리별로 가격의 평균과 최소값을 계산**하는 쿼리입니다.
```sql
SELECT category, AVG(price) AS average_price, MIN(price) AS min_price
FROM products
GROUP BY category;
```

예시 4) orders 테이블에서 **날짜별로 주문 수와 주문 총액을 계산**하는 쿼리입니다.
```sql
SELECT DATE(order_date) AS order_date, COUNT(*) AS order_count, SUM(order_amount) AS total_amount
FROM orders
GROUP BY DATE(order_date);
```

#### GROUP BY 변수가 두개일때
- 예시
```
CAR_RENTAL_COMPANY_RENTAL_HISTORY:

HISTORY_ID  CAR_ID  START_DATE  END_DATE
1           1       2022-08-01   2022-08-05
2           1       2022-08-10   2022-08-12
3           1       2022-09-01   2022-09-05
4           2       2022-08-03   2022-08-07
5           2       2022-08-12   2022-08-15
6           2       2022-09-02   2022-09-06
```

1. 첫 번째 쿼리
```sql
SELECT month(START_DATE) AS MONTH, CAR_ID, COUNT(*) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE START_DATE >= '2022-08-01' AND START_DATE < '2022-11-01'
GROUP BY
    MONTH,
    CAR_ID
```

```
MONTH  CAR_ID  RECORDS
8      1       2
8      2       2
9      1       1
9      2       1
```
=> 첫 번째 쿼리에서 월별 정보를 제공하므로, 각 월별로 어떤 자동차가 얼마나 대여되었는지 알 수 있습니다.

2. 두 번째 쿼리
```sql
SELECT month(START_DATE) AS MONTH, CAR_ID, COUNT(*) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE START_DATE >= '2022-08-01' AND START_DATE < '2022-11-01'
GROUP BY
    CAR_ID
```

```
MONTH  CAR_ID  RECORDS
8      1       3
8      2       3
```
=> 두 번째 쿼리는 월별 정보를 고려하지 않고 자동차별 전체 대여 횟수를 반환합니다.


#### GROUP BY 틀린문제
1) [코딩테스트 연습 - 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/157340)
- 어떤 부분에서 틀렸을까??
```sql
SELECT CAR_ID, START_DATE, END_DATE,
       CASE
         WHEN START_DATE < '2022-10-16' AND END_DATE < '2022-10-16' THEN '대여 가능'
         WHEN START_DATE > '2022-10-16' AND END_DATE > '2022-10-16' THEN '대여 가능'
         ELSE '대여중'
       END AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
ORDER BY CAR_ID DESC;
```
- 이 부분을 GROUP BY로 어떻게 표현할까??
![[Pasted image 20230517173942.png]]

2) [코딩테스트 연습 - 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/151139)
- 총 대여 횟수(RECODS)가 5회 이상인 자동차이지만, 월이랑 총은 다른데 어떻게 표현할까??
![[Pasted image 20230517181757.png]]
- 밑에 SQL을 위에 SQL 조건문에 어떻게 넣을까??
```sql
SELECT month(START_DATE) AS MONTH, CAR_ID, COUNT(*) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE year(START_DATE) = 2022 AND month(START_DATE) BETWEEN 8 AND 10
GROUP BY CAR_ID
ORDER BY month(START_DATE), CAR_ID DESC


SELECT COUNT(*)
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE year(START_DATE) = 2022 AND month(START_DATE) BETWEEN 8 AND 10
GROUP BY CAR_ID
HAVING COUNT(*) >= 5
```


## HAVING 문
#### HAVING 문이란?
- **집계함수의 조건문** (WHERE에서는 사용 불가) why? WHERE문이 GROUP보다앞이라서 적용 X

예시) **회원별로 총 구매 금액을 계산후, 그 중 총 구매 금액이 1000보다 큰 회원들을 선택**하는 쿼리입니다.
```sql
SELECT mem_id "회원 아이디", SUM(price*amount) "총 구매 금액"  
FROM buy  
GROUP BY mem_id  
HAVING SUM(price*amount) > 1000;
```


## JOIN
#### INNER JOIN
![[Pasted image 20230514131606.png]]
INNER JOIN은 **두 테이블에서 일치하는 행만 반환**합니다. 일치하는 행이 없으면 결과에 표시되지 않습니다.
```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

데이터가 다음과 같은 경우를 가정하면

Orders 테이블
```
OrderID | CustomerID
--------|-----------
1       | 100
2       | 101
3       | 100
```

Customers 테이블
```
CustomerID | CustomerName
-----------|-------------
100        | Alice
101        | Bob
```

이 쿼리를 실행하면 다음 결과 집합을 얻을 수 있습니다:
```
OrderID | CustomerName
--------|-------------
1       | Alice
2       | Bob
3       | Alice
```


#### LEFT JOIN (또는 LEFT OUTER JOIN)
![[Pasted image 20230514131624.png]]
**LEFT JOIN은 왼쪽 테이블의 모든 행을 반환**하며, 오른쪽 테이블에서 일치하는 행이 있는 경우 해당 행이 결합되어 표시됩니다. 일치하는 행이 없으면 NULL 값이 반환됩니다.
```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

데이터가 다음과 같은 경우를 가정하면

Customers 테이블
```
CustomerID | CustomerName
-----------|-------------
1          | Alice
2          | Bob
3          | Carol
```

Orders 테이블
```
OrderID | CustomerID
--------|-----------
1001    | 1
1002    | 2
1003    | 2
```

이 쿼리를 실행하면 다음 결과 집합을 얻을 수 있습니다:
```
CustomerName | OrderID
-------------|--------
Alice        | 1001
Bob          | 1002
Bob          | 1003
Carol        | NULL
```


#### letf join에서 테이블이 왼쪽인지 오른쪽인지는 어떻게 정할까?
=>왼쪽 테이블과 오른쪽 테이블을 선택하는 것은 주로 결과 집합에 **어떤 데이터를 포함하고 싶은지에 따라 결정(FROM 부분)** 됩니다.

예를 들어, 직원 정보가 있는 `employees` 테이블과 부서 정보가 있는 `departments` 테이블이 있을 때, 모든 직원의 정보를 결과에 포함시키고 싶다면(FROM 부분) `employees` 테이블을 왼쪽 테이블로 선택합니다.
```sql
SELECT a.id, a.name, b.department
FROM employees a
LEFT JOIN departments b ON a.department_id = b.id;
```
이 경우, `employees` 테이블이 왼쪽 테이블이고 `departments` 테이블이 오른쪽 테이블입니다.


#### RIGHT JOIN (또는 RIGHT OUTER JOIN)
![[Pasted image 20230514131637.png]]
RIGHT JOIN은 오른쪽 테이블의 모든 행을 반환하며, 왼쪽 테이블에서 일치하는 행이 있는 경우 해당 행이 결합되어 표시됩니다. 일치하는 행이 없으면 NULL 값이 반환됩니다.
```sql
SELECT Orders.OrderID, Employees.LastName, Employees.FirstName  
FROM Orders  
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID  
ORDER BY Orders.OrderID;
```

데이터가 다음과 같은 경우를 가정하면

Orders 테이블:
```
OrderID | EmployeeID
--------|-----------
1       | 1
2       | 2
3       | 3
```

Employees 테이블:
```
EmployeeID | LastName | FirstName
-----------|----------|----------
1          | Smith    | John
2          | Doe      | Jane
3          | Brown    | James
4          | Johnson  | Jennifer
```

이 쿼리를 실행하면 다음 결과 집합을 얻을 수 있습니다:
```
OrderID | LastName | FirstName
--------|----------|----------
1       | Smith    | John
2       | Doe      | Jane
3       | Brown    | James
NULL    | Johnson  | Jennifer
```


#### left join 있는데 right join 쓰는 이유는?
LEFT JOIN과 RIGHT JOIN은 기본적으로 **동일한 기능을 수행**하지만, 결합되는 테이블의 **순서에 따라 결과가 달라집**니다. 

1) 개발자의 가독성 선호도
쿼리를 작성하는 개발자가 RIGHT JOIN을 사용하면 코드를 더 쉽게 이해하고 작성할 수 있다고 느낄 수 있습니다.

2) 쿼리의 순서에 따른 가독성
쿼리의 목적이나 복잡도에 따라 RIGHT JOIN을 사용하여 쿼리를 더 읽기 쉽게 만들 수 있습니다. 예를 들어, 여러 테이블을 결합하는 경우에 RIGHT JOIN을 사용하면 중요한 정보가 먼저 표시되는 등의 가독성이 향상될 수 있습니다.

3) 기존 쿼리 수정
기존 쿼리를 수정하거나 개선할 때, 다른 JOIN 형식을 사용하면 쿼리 구조를 크게 변경해야 할 수도 있습니다. 이러한 경우 RIGHT JOIN을 사용하여 쿼리를 유지하고 필요한 데이터만 추가할 수 있습니다.


#### CROSS JOIN
![[Pasted image 20230514131653.png]]
CROSS JOIN은 왼쪽 테이블의 모든 행과 오른쪽 테이블의 모든 행 사이에 카테시안 곱을 반환합니다. 즉, 왼쪽 테이블의 각 행이 오른쪽 테이블의 각 행과 결합되어, **가능한 모든 조합이 결과 집합에 포함**됩니다.

```sql
SELECT Customers.CustomerName, Orders.OrderID 
FROM Customers 
CROSS JOIN Orders;
```

데이터가 다음과 같은 경우를 가정해 보면

Customers 테이블
```
CustomerID | CustomerName
-----------|-------------
1          | Alice
2          | Bob
3          | Carol
```

Orders 테이블
```
OrderID | CustomerID
--------|-----------
1001    | 1
1002    | 2
1003    | 2
```

이 쿼리를 실행하면 다음 결과 집합을 얻을 수 있습니다:
```
CustomerName | OrderID
-------------|--------
Alice        | 1001
Alice        | 1002
Alice        | 1003
Bob          | 1001
Bob          | 1002
Bob          | 1003
Carol        | 1001
Carol        | 1002
Carol        | 1003
```


#### CROSS JOIN을 사용하는 이유는?
CROSS JOIN은 두 테이블 간의 **모든 가능한 조합을 생**성하는 데 사용되는 조인 유형입니다

1) 조합 생성: 두 테이블 간에 **모든 가능한 조합을 생성해야 하는 경우**에 유용합니다. 예를 들어, 제품과 색상이 각각 별도의 테이블에 저장되어 있는 온라인 쇼핑몰에서 사용 가능한 모든 제품 색상 조합을 생성하려면 CROSS JOIN을 사용할 수 있습니다.

2) 테스트 데이터 생성: 개발 및 테스트 과정에서 **대량의 테스트 데이터가 필요한 경우**, CROSS JOIN을 사용하여 다양한 경우의 수를 생성할 수 있습니다. 이렇게 하면 빠르게 많은 수의 테스트 데이터를 만들어 낼 수 있어 테스트 과정을 효율적으로 진행할 수 있습니다.

3) 행 복제: 한 테이블의 행을 복제하거나 **특정 횟수만큼 반복해야 할 때** CROSS JOIN을 사용할 수 있습니다. 예를 들어, 특정 행을 5번 복제하려면 다른 테이블에 5개의 더미 행이 있는 경우 CROSS JOIN을 사용하여 원하는 결과를 얻을 수 있습니다.
