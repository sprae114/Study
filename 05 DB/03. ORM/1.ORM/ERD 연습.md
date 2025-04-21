
#### Order
- 단독 order CRUD 
- member와 N:1 양방향 연결되어 있어서 
```
1) 생성 후 member와 연결시켜 조회
2) 여러개의 member와 연결시켜 조회해보기
3) 기존의 order가 갖고있는 fk를 다른 member_id로 변경시켜보기
4) 주문 삭제 시켜보기
```

#### Member
- 단독 member CRUD
- 양방향이니깐 memberRepository로 해당 member가 가지고 있는 order확인해보기(조회만 가능함.)

## 수정
- 코드
```java
@DisplayName("FK 수정 : 연관관계 끊기 성공")  
@Test  
void updateOrders4() {  
    //given  
    Member member2 = createMember("member2", "city2", "street2", "zipcode2");  
  
    // 기존 member1의 연관관계 orders1을 삭제한다.  
    member.removeOrder(orders1);  
  
    // orders1에 member2 연관관계를 설정한다.  
    orders1.setMember(member2);  
    ordersRepository.save(orders1);  
  
    entityManager.clear();  
  
    //when  
    System.out.println("=====================================");  
    memberRepository.findAll().forEach(System.out::println);  
    System.out.println("=====================================");  
    ordersRepository.findAll().forEach(System.out::println);  
    System.out.println("=====================================");  
    Member member1 = memberRepository.findByName("member1").get();  
  
    //then  
    Assertions.assertThat(member1.getOrders()).hasSize(1);  
}
```

```java
Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=[Orders(id=1, orderDate=2021-08-01, status=ORDER1), Orders(id=2, orderDate=2021-08-02, status=ORDER2)])

Member(id=2, name=member2, city=city2, street=street2, zipcode=zipcode2, orders=[])
```

```java
Orders(id=1, member=Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-01, status=ORDER1)

Orders(id=2, member=Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```


▶ flush 이후
```java
Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=[Orders(id=2, orderDate=2021-08-02, status=ORDER2)])

Member(id=2, name=member2, city=city2, street=street2, zipcode=zipcode2, orders=[Orders(id=1, orderDate=2021-08-01, status=ORDER1)])
```


```java
Orders(id=1, member=Member(id=2, name=member2, city=city2, street=street2, zipcode=zipcode2), orderDate=2021-08-01, status=ORDER1)

Orders(id=2, member=Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```

▶ removeOrder 삭제하면?
```java
Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=[Orders(id=2, orderDate=2021-08-02, status=ORDER2)])

Member(id=2, name=member2, city=city2, street=street2, zipcode=zipcode2, orders=[Orders(id=1, orderDate=2021-08-01, status=ORDER1)])
```


```java
Orders(id=1, member=Member(id=2, name=member2, city=city2, street=street2, zipcode=zipcode2), orderDate=2021-08-01, status=ORDER1)

Orders(id=2, member=Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```

## 삭제
#### 실패 : clear()만 할 경우
```java
Member(id=3, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=[Orders(id=5, orderDate=2021-08-01, status=ORDER1), Orders(id=6, orderDate=2021-08-02, status=ORDER2)])
```


```java
Orders(id=5, member=Member(id=3, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-01, status=ORDER1)
Orders(id=6, member=Member(id=3, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```


#### 실패 : flush()만 할 경우
```java
Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=null)
```


```java
Orders(id=2, member=Member(id=1, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```


#### 성공 : clear() + flush() 
```java
Member(id=2, name=member1, city=city1, street=street1, zipcode=zipcode1, orders=[Orders(id=4, orderDate=2021-08-02, status=ORDER2)])
```


```java
Orders(id=4, member=Member(id=2, name=member1, city=city1, street=street1, zipcode=zipcode1), orderDate=2021-08-02, status=ORDER2)
```

