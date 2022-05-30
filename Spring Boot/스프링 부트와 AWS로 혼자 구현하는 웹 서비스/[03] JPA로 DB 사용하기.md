### 데이터베이스 다루기

- 과거에는 MyBatis와 같은 SQL Mapper를 이용해 쿼리를 작성. 이는 테이블 모델링에 집중하고 객체 지향적이지 못함.
- 해결책으로 JPA라는 자바 표준 ORM(Object Relational Mapping)
- MyBatis, iBatis는 ORM이 아닌 SQL Mapper. ORM은 객체를 매핑하는 것. SQL Mapper는 쿼리를 매핑하는 것.

#### SQL 사용의 문제점

- 객체를 관계형 데이터 베이스에서 관리하는 것은 매우 중요
- SQL을 통해야만 DB에 저장하고 조회할 수 있다. 기본적인 CRUD를 매번 생성해야 한다.
- 첫째, 이는 너무 반복적임
- 들쩨, 패러다임 불일치
  - 관계쳥 DB: 어떻게 데이터를 저장할지
  - 객체지향 프로그래밍 언어: 메시지를 기반으로 기능과 속성을 한 곳에서 관리
- JPA는 중간에서 패러다임 일치를 시켜준다.
- 개발자는 객체지향적으로 프로그래밍을 한다 -> JPA가 이를 관계형 DB에 맞게 SQL을 대신 생성해 실행.
  - => 더는 SQL에 종속적인 개발을 하지 않을 수 있다.

### Spring Data JPA

- JPA는 인터페이스로 사용하기 위해서는 구현체가 필요한데 대표적으로 Hibernate, Eclipse Link 등이 있는데 Spring에서는 Spring Data JPA를 사용한다.
- 장점
    - 구현체 교체의 용이성: Hibernate 외에 다른 구현체로 쉽게 교체하기 위함.
      Spring Data JPA 내부에서 구현체 매핑을 지원해 준다.
    - 저장소 교체의 용이성: 관계형 DB 외에 다른 저장소로 쉽게 교체 가능
      트래픽이 증가함에 따라 관계형 DB로 처리가 안되어 MongoDB로 교체가 필요하다면 Spring Data JPA에서는 의존성만 교체하면 된다.
      Spring Data의 하위 프로젝트들이 기본적인 CRUD의 인터페이스가 같기 때문.
      
### Spring Data JPA 적용하기

1️⃣ build.gradle에 의존성 등록
```java
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // 스프링 부트용 Spring Data Jpa 추상화 라이브러리
    implementation 'com.h2database:h2' // 인메모리 관계형 데이터베이스. 재시작마다 초기화되어 테스트 용도로 사용
}
```

✳ domain이란?
- 소프트웨어에 대한 요구사항 혹은 문제 영역

2️⃣ 도메인 생성

```java
@Getter
@NoArgsConstructor // 기본 생성자
@Entity // 테이블과 링크될 클래스
public class Posts {
    
    @Id // 테이블의 PK
    @GeneratedValue(strategy = GenerationType.IDENTITY) // PK의 생성 규칙 auto_increment를 위해
    private Long id;
    
    @Column(length = 500, nullable = false) // 컬럼 추가 설명
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;
    
    private String author;

    @Builder // 해당 클래스의 빌더 패턴 클래스 생성
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```

✳ Setter
- Entity 클래스에서는 절대 Setter 메소드를 만들지 않는다.
- 필요하면 *명확히* 그 목적과 의도를 나타낼 수 있는 메소드를 추가해야함.
- 여기서는 생성자를 대신해 Builder를 사용
  - Builder는 채워야 할 필드가 무엇인지 명확히 지정할 수 있다.

❌ 생성자 사용
```
public Example(String a, String b){
  this.a = a;
  this.b = b;
}
```
⭕ 빌더 
```
Example.builder()
  .a(a)
  .b(b)
  .build()
```

💔 한글로 작성된 함수명이 commit 되지 않는다

<img src="https://user-images.githubusercontent.com/60870438/170870906-086a0239-2891-4af6-9f55-9fbd2fc1ba24.png" width=50%>

Editor > Inspections > Internationalization > Non-Ascii characters in Identifiers & Different languages in identifiers 체크 해제하기

<img src="https://user-images.githubusercontent.com/60870438/170870948-e6780054-ecaa-42f6-b5a7-547e8d8c12ed.png" width=50%>

💖 해결 완료!

3️⃣ Jpa Repository interface 생성하기
- DB Layer 접근자로 JPA에선 Repository라고 부르며 인터페이스로 생성. 기본적인 CRUD가 자동 생성된다.
- @Repository를 추가할 필요가 없으나 Entity 클래스와 기본 Entity Repository는 *함께* 위치해야 한다.

#### Spring Data JPA 테스트 코드

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class PostsRepositoryTest {
    
    @Autowired
    PostsRepository postsRepository;
    
    @After
    public void cleanup(){
        postsRepository.deleteAll();
    }
    
    @Test
    public void 게시글저장_블러오기(){
        //given
        String title = "테스트 게시글";
        String content = "테스트 본문";
        
        postsRepository.save(Posts.builder() 
                .title(title)
                .content(content)
                .author("minju@gmail.com")
                .build());
        
        //when
        List<Posts> postsList = postsRepository.findAll();
        
        //then
        Posts posts = postsList.get(0);
        assertThat(posts.getTitle()).isEqualTo(title);
        assertThat(posts.getContent()).isEqualTo(content);
    }
}
```

✳ @After
  - Junit에서 단위 테스트가 끝날 때마다 수행되는 메소드 지정
  - 전체 테스트 수행시 테스트간 데이터 침범을 막기 위해 사용
 
✳ save
  - 테이블 posts에 insert/update 실행
  - id 값이 있다면 update, 없다면 insert

✳ findAll
  - 테이블 posts에 있는 모든 데이터 조회

✳ @SpringBootTest
  - 별다른 설정없이 사용하면 H2 DB를 자동으로 실행한다.

✳ applications.properties
```
spring.jpa.show-sql=true // 생성되는 SQL문 확인하기
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect // H2 쿼리 문법이 아닌 MySQL 쿼리 문법을 사용하도록 변경
```

#### API 만들기

<img src="https://user-images.githubusercontent.com/60870438/170873733-5c8a6579-ffe5-46a7-aede-4a0bec0ff458.png" width=50%>

1. Web Layer
  - 흔히 사용하는 @Controller 등의 뷰 템플릿 영역
  - 외부 요청과 응답에 대한 전반적인 영역
2. Service Layer
  - @Service에 사용되는 서비스 영역
  - @Transactional이 사용
3. Repository Layer
  - DB와 같이 데이터 저장소에 접근하는 영역
4. Dtos
  - 계층간 데이터 교환을 위한 객체
5. Domain Model
  - 도메인이라 불리는 개발 대상을 단순화시킨 것
  - @Entity

> 비즈니스 로직을 처리해야할 곳은?
>   Domain ⁉
>   - 서비스에서는 트랜잭션과 도메인 간의 순서만 보장해주고 도메인 내에 메서드를 구현한다.

#### Dto를 만드는 방법

```
@Getter
@NoArgsConstructor
public class PostsSaveRequestDto {

    private String title;
    private String content;
    private String author;

    @Builder // 빌더
    public PostsSaveRequestDto(String title, String content, String author){
        this.title = title;
        this.content = content;
        this.author = author;
    }

    public Posts toEntity(){ // 생성자를 여기서 만들어준다.
        return Posts.builder()
                .title(title)
                .content(content)
                .author(author)
                .build();
    }
}
```

✳ JPA 영속성 컨텍스트
  - 모든 request 혹은 response는 dto를 생성한다. 
  - Service의 일부분
```
    @Transactional
    public Long update(Long id, PostsUpdateRequestDto requestDto) {
        Posts posts = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));

        posts.update(requestDto.getTitle(), requestDto.getContent());

        return id;
    }
```
- update 기능에서 쿼리를 날리지 않는다. 이유는 JPA의 영속성 컨텍스트 때문이다. 이는 엔티티를 영구 저장하는 환경.
- 트랜잭션 안에서 DB에서 데이터를 가져오면 해당 데이터는 영속성 컨텍스트가 유지된 상태
- 이 상태에서 값을 변경하면 트랜잭션이 끝나는 시점에서 해당 테이블에 변경분을 반영한다.
- 이를 *더티체킹*이라고 한다. 

#### 조회는 톰캣을 실행해보자
application.properties에 추가
```
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:testdb // h2 url 지정
```

<img src="https://user-images.githubusercontent.com/60870438/170879250-b0043f51-e607-4f02-a384-70bfe7a7dce7.png" width=50%>

### JPA Auditing으로 생성시간.수정시간 자동화하기

1️⃣ BaseTimeEntity 생성
  - BaseTimeEntity 클래스는 모든 Entity의 상위 클래스가 되어 Entity들의 createdDate, modifiedDate를 자동으로 관리하는 역할을 한다.
```
@Getter
@MappedSuperclass 
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {
    
    @CreatedDate
    private LocalDateTime createDate;
    
    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

✳ @MappedSuperclass
  - JPA Entity 클래스들이 BaseTimeEntiry를 상송할 경우 필드들도 컬럼으로 인식하도록 지정

✳ @EntityListeners
  - BaseTimeEntity 클래스에 Auditing 기능을 포함시킨다.

✳ @CreatedDate
  - Entity가 생성되어 저장될 때 시간이 자동 저장

✳ @LastModifiedDate
  - 조회한 Entity의 값을 변경할 때 시간이 자동 저장

2️⃣ 필요한 Entity 클래스(Posts)가 BaseTimeEntity를 상속받게 한다.
```
public class Posts extends BaseTimeEntity {
```

3️⃣ JPA Auditing 어노테이션을 모두 활성화할 수 있도록 Application 클래스에 활성화 어노테이션 하나 추가
```
@EnableJpaAuditing // JPA Auditing 활성화
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
```

4️⃣ 테스트 코드 작성
```
    @Test
    public void BaseTimeEntity_등록(){
        //given
        LocalDateTime now = LocalDateTime.of(2022,5,30,0,0,0);
        postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());

        //when
        List<Posts> postsList = postsRepository.findAll();

        //then
        Posts posts = postsList.get(0);

        System.out.println(">>>>>>>>>>>>>>>> createDate="+posts.getCreateDate()+", modifiedDate="+posts.getModifiedDate());
        assertThat(posts.getCreateDate()).isAfter(now);
        assertThat(posts.getModifiedDate()).isAfter(now);
    }
```
