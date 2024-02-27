# 사전 정리
✔유저(학생, 선생님, 관리자) : user
- id
- 이름 : name
- 아이디 : email
- 비번 : password
- 권한 : authorities
- 학년 : grade
- 담임 : teacher (선생님 값은 null)
- 학교 : school


✔학교 : school
- id
- 도시 : city
- 학교 이름 : name
(teacherCount : 선택한 선생수, studyCount : 선택한 학생수)


✔권한 : authority
- id : 유저 id
- 권한 : authority


✔문제집 : papertemplate
- id
- 이름 : name
- 만든이 : creator
- 만든이 id : userId
- total : 만든 문제 수
- created
- updated


✔문제 : problem
- id
- 문제 내용 : content
- 정답 : answer
- 문제 순서 : index_num
- 어디 문제집인지 : paper_template_id


✔시험지 : paper
- id
- 어디 문제집인지 : paper_template_id
- 시험지 이름 : name
- 시험 본 학생 : study_user_id


✔제출한 답변 : paper_answer
- 정답 : answer
- 제출시간 : answered
- 채점결과 : correct
- 문제집 번호 : paper_id
- 문제번호 

# comp
## paper
#### Paper

1. paperId: 문서의 고유 ID
2. paperTemplateId: 시험지 ID
3. name: 시험지의 이름
4. studyUserId: 학생 ID
5. user: User 객체 (Transient로 설정되어 DB에 저장되지 않습니다.)
6. paperAnswerList: 시험 리스트 입니다.(OneToMany 관계)
7. created, startTime, endTime : 각각 문서가 만들어진 시간, 시작 시간 및 종료 시간
8. state : PaperState 타입으로 현재 상태(READY / START / END / CANCELED)
9. 9.total : 총 점수
10. answered : 답변한 점수
11. correct : 정답인 점수






## paperUser



## userAdmin


# web
## paperApp


## siteManager


## siteStudent


## siteTeacher


# 01-1. 학교 도메인 설계및 테스트

#### @Query작성하는 방법은?

#### springDataJpa에서 메소드 이름을 작성하는 방법은?

#### 람다식 어떻게 나왔을까?

```java
Optional<School> optionalSchool = schoolRepository.findById(schoolId);
if (optionalSchool.isPresent()) {
	School school = optionalSchool.get();
	school.setName(name);
	schoolRepository.save(school);
}
return optionalSchool;
```

```java
return schoolRepository.findById(schoolId).map(school -> {
        school.setName(name);
        schoolRepository.save(school);
        return school;
    });
```

# 01-2. 선생님-학생도메인 설계

#### `DAO Authentication Provider`이란?
[[06. DB와 연동하기]]

**Spring Security에서 사용되는 인증 메커니즘**을 가리킵니다. DAO(Data Access Object)는 데이터베이스에 접근하는 객체를 말하며, 이를 활용하여 사용자의 인증 정보를 관리합니다.

`DAO Authentication Provider`는 `UserDetails` 인터페이스를 구현한 객체를 통해 데이터베이스에서 사용자 정보를 가져옵니다. 이 정보에는 사용자 이름, 비밀번호, 권한 등이 포함됩니다. `UserDetails` 서비스가 제공하는 정보와 클라이언트가 제공하는 자격 증명(예: 비밀번호)을 비교하여 인증을 수행합니다.

`UserDetails` 서비스 구현체의 예로 `UserDetailsService`가 있습니다. 이 서비스는 `loadUserByUsername()` 메서드를 통해 특정 사용자의 세부 사항을 로드할 수 있습니다.

따라서 `DAO Authentication Provider`란, 실제 데이터베이스에 저장된 사용자 정보와 클라이언트가 제공한 자격 증명을 비교하여 인증 과정을 수행하는 `Spring Security`의 컴포넌트입니다.


#### DAO, DTO, VO,Entity 차이는?
1. DAO (Data Access Object): **데이터베이스의 데이터에 접근을 추상화**하고 캡슐화한 객체입니다. CRUD(Create, Read, Update, Delete) 같은 기본적인 데이터베이스 연산 메서드를 제공합니다. 이를 통해 데이터베이스 접근 로직과 비즈니스 로직을 분리할 수 있습니다.

2. DTO (Data Transfer Object): 계층간 데이터 교환을 위한 객체입니다. 일반적으로 소프트웨어의 다른 부분으로 **전달될 목적으로 정보를 담기 위해 사용**됩니다. 간단한 구조체나 컨테이너로 볼 수 있으며 보통 getter와 setter 메서드를 가지고 있습니다.

3. VO (Value Object): 값 자체가 동일성을 가지는 객체입니다. 즉 값이 같으면 두 객체도 같다고 판단합니다(예: 돈 10달러). 일반적으로 변경 불가능(immutable)하게 설계되며 비즈니스 로직에서 의미있는 정보를 표현하는데 사용됩니다.

4. Entity: **데이터베이스 테이블 내에 있는 하나의 레코드 혹은 행을 나타내는 클래스**입니다(JPA에서 주로 사용). 각 인스턴스는 DB의 한 row에 해당하며 식별자(ID)에 의해 구분됩니다.


ㅁㅁ Authority를 연관관계 하지않고 하나의 테이블로 해결할 순 없을까?

```java
public class User implements UserDetails {
	
	...
	
	@OneToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)  
	@JoinColumn(foreignKey = @ForeignKey(name = "userId"))  
	private Set<Authority> authorities;

	...
}
```


```java
@OneToOne  
private Authority authority;  
  
  
@Override  
public Collection<? extends GrantedAuthority> getAuthorities() {  
    return authority != null ? Collections.singletonList(authority) : Collections.emptyList();  
}
```



# 01-3. 시험지 템플릿 도메인



# 02-1 메인화면과 로그인화면



# 02-2 학생-선생님-관리자 사이트 화면



# 02-3 사이트 상세화면 만들기



# 03-1 사이트 권한 코딩(1)



# 03-2 사이트 권한 코딩(2)
