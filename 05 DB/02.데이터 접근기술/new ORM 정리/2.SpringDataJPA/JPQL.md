JPQL(Java Persistence Query Language)은 SQL을 기반으로 한, 엔티티 객체 모델을 사용하여 쿼리를 작성하는 언어입니다. SQL과 비슷하지만, 테이블 대신 엔티티 객체를 대상으로 쿼리를 작성합니다.

1. **기본 문법**
JPQL의 기본적인 문법은 SQL과 매우 유사합니다. SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 등의 키워드를 사용할 수 있습니다.

-  "salary가 50000보다 큰 모든 Employee 엔티티"를 선택하는 쿼리
```java
SELECT e FROM Employee e WHERE e.salary > 50000
```


2. **JOIN**
JPQL에서는 엔티티 간의 관계에 따라 JOIN을 수행할 수 있습니다. INNER JOIN, LEFT (OUTER) JOIN 등을 지원합니다.

- "department가 'Sales'인 모든 Employee 엔티티"를 선택하는 쿼리
```java
SELECT e FROM Employee e JOIN e.department d WHERE d.name = 'Sales'
```


3. **Named Parameters**
JPQL에서는 이름이 붙은 매개변수(named parameters) 또는 위치 기반 매개변수(positional parameters)를 사용하여 동적인 값을 바인딩할 수 있습니다.

- ":salary"라는 named parameter에 실제 값을 바인딩하는 예시
```java
String jpql = "SELECT e FROM Employee e WHERE e.salary > :salary";
Query query = em.createQuery(jpql);
query.setParameter("salary", 50000);
List<Employee> results = query.getResultList();
```


4. **Aggregate Functions**
COUNT(), SUM(), AVG(), MAX(), MIN() 등의 집계 함수를 지원합니다.

```java
// Employee의 평균 salary를 계산합니다.
String jpql = "SELECT AVG(e.salary) FROM Employee e";
Double avgSalary = em.createQuery(jpql, Double.class).getSingleResult();

// Department별로 최대 salary를 계산합니다.
jpql = "SELECT d.name, MAX(e.salary) FROM Department d JOIN d.employees e GROUP BY d.name";
List<Object[]> results = em.createQuery(jpql).getResultList();
for (Object[] result : results) {
    String departmentName = (String) result[0];
    Double maxSalary = (Double) result[1];
    // ...
}
```


5. **Subqueries**
서브쿼리(subquery) 를 지원하며 이들은 다른 쿼리 내부에서 사용됩니다.

```java
// 각 Department의 평균 salary보다 많은 salary를 받는 Employee들을 조회합니다.
jpql = "SELECT e FROM Employee e WHERE e.salary > (SELECT AVG(e2.salary) FROM Employee e2 WHERE e2.department = e.department)";
List<Employee> employees = em.createQuery(jpql, Employee.class).getResultList();
```



6. **UPDATE and DELETE** : 
SELECT 외에도 UPDATE와 DELETE문도 지원하나 이들은 복잡한 조건이나 조인을 포함하지 않아야 합니다.

```java
// 모든 Employee의 salary를 10% 인상합니다.
em.createQuery("UPDATE Employee e SET e.salary = e.salary * 1.1").executeUpdate();

// salary가 10000 미만인 모든 Employee를 삭제합니다.
em.createQuery("DELETE FROM Employee e WHERE e.salary < 10000").executeUpdate();
```


7. **JPQL의 결과 타입**
JPQL 쿼리의 결과는 일반적으로 엔티티 또는 임베디드 타입의 객체입니다. 하지만, SELECT 절에서 특정 필드만 선택하는 경우 각 필드 값의 배열(Object[])이나 사용자 정의 결과 클래스를 반환할 수도 있습니다.

```java
// Object[] 반환
List<Object[]> results = em.createQuery("SELECT e.name, e.salary FROM Employee e").getResultList();

// 사용자 정의 결과 클래스 반환
List<EmployeeDto> results = em.createQuery("SELECT new com.example.EmployeeDto(e.name, e.salary) FROM Employee e", EmployeeDto.class).getResultList();
```


8. **다형성**
JPQL은 다형성을 지원합니다. 상위 클래스에 대한 쿼리를 실행하면, 해당 상위 클래스와 그 하위 클래스의 모든 인스턴스가 쿼리 결과로 포함됩니다.

```java
// 'Person' 엔티티와 그 하위 엔티티('Employee', 'Customer' 등)가 모두 조회됨
List<Person> people = em.createQuery("SELECT p FROM Person p", Person.class).getResultList();
```


9. **엔티티 식별자 접근**
엔티티 식별자에 접근하기 위해 `TYPE` 및 `TREAT` 키워드를 사용할 수 있습니다.

```java
// 'Employee' 타입인 'Person' 엔티티만 조회함
List<Employee> employees = em.createQuery("SELECT p FROM Person p WHERE TYPE(p) = Employee", Employee.class).getResultList();

// 부모 클래스 참조를 자식 클래스 참조로 다운캐스트하여 속성에 접근함 
String jpql = "SELECT e.salary FROM Person p WHERE TREAT(p AS Employee).salary > 50000";
```


10. **FETCH JOIN**
JPQL에서는 FETCH JOIN을 이용하여 연관된 엔티테들을 한 번에 로딩할 수 있습니다. 이렇게 하면 N+1 문제를 효과적으로 해결할 수 있습니다.

```java
String jpql = "SELECT d FROM Department d JOIN FETCH d.employees";
```


11. **벌크 연산**
JPQL은 UPDATE, DELETE와 같은 벌크 연산을 지원합니다. 이를 통해 한 번의 쿼리로 여러 행을 수정하거나 삭제할 수 있습니다. 단, 벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 직접 쿼리를 실행하기 때문에 주의가 필요합니다.

```java
// 모든 Employee의 salary를 10% 인상
em.createQuery("UPDATE Employee e SET e.salary = e.salary * 1.1").executeUpdate();

// salary가 10000 미만인 Employee를 삭제
em.createQuery("DELETE FROM Employee e WHERE e.salary < 10000").executeUpdate();
```


12. **컬렉션 표현식**
`IS EMPTY`, `MEMBER OF`, `SIZE` 등의 키워드로 컬렉션을 다룰 수 있습니다.

```java
// employees가 비어있는 Department 조회
List<Department> emptyDepartments = em.createQuery("SELECT d FROM Department d WHERE d.employees IS EMPTY", Department.class).getResultList();

// 특정 Employee가 속한 Department 조회
Employee employee = em.find(Employee.class, id);
List<Department> departments = em.createQuery("SELECT d FROM Department d WHERE :employee MEMBER OF d.employees", Department.class)
    .setParameter("employee", employee)
    .getResultList();

// employees의 크기(수)에 따른 Department 조회
List<Department> largeDepartments = em.createQuery("SELECT d FROM Department d WHERE SIZE(d.employees) > 10", Department.class).getResultList();
```


13. **동적 쿼리**: `Criteria API` 또는 `Querydsl`과 같은 도구를 사용하면 JPQL 문자열을 동적으로 생성하지 않고도 타입-세이프한 방식으로 동적 쿼리를 작성할 수 있습니다.