스프링 시큐리티(Spring Security)는 막강한 인증(Authentication)과 인가/권한 부여(Authorization)의 기능을 가진 프레임워크

### 스프링 시큐리티와 스피링 시큐리티 Oauth2

- 스프링 부트 1.5 방식에서는 url 주소를 모두 명시해야 하지만, 2.0 방식에서는 client 인증 정보만 입력하면 된다.
- 1.5에서  직접 입력했던 값들이 2.0버전에서는 enum으로 대체되었다.
- CommonOAuth2Provider이라는 enum이 새롭게 추가되어 구글, 깃허브, 페이스북, 옥타의 기본 설정값을 제공한다. 이외에 다른 소셜 로그인이 필요하다면 직접 추가해야 한다.

#### 1️⃣ 구글 서비스 등록

1. https://console.cloud.google.com 이동
2. 새프로젝트 생성
3. 메뉴의 API 밑 서비스 > 사용자 인증 정보 > 만들기
4. OAuth 클라이언트 ID 만들기 > 동의 화면 구성
5. 승인된 리디렉션 URI 채우기
  - 서비스에서 파라미터로 인증 정보를 주었을 때 인증이 성공하면 구글에서 리다이렉트할 URL
  - 2.0버전에서는 기본적으로 {도메인}/login/oauth2/code/{소셜 서비스 코드}

#### 2️⃣ application-oauth.properties 설정
```
spring.security.oauth2.client.registration.google.client-id={클라이언트 ID}
spring.security.oauth2.client.registration.google.client-secret={클라아언트 비밀번호}
spring.security.oauth2.client.registration.google.scope=profile,email
```
- scope의 기본값이 profile, email, openid
- openid가 있으면 Open Id Provider로 인식한다. -> OpenId Provider인 구글과 그렇지 않은 서비스(네이터/카카오 등)으로 나눠서 OQuth2Service를 만들어야 한다.
- 하나의 OAuth2Service로 사용하기 위해 openid scope 제외

✳ 스프링 부트에서 properties의 이름을 appication-xxx.properties로 만들면 xxx라는 이름의 profile이 생성되어 관리할 수 있다.
- 즉 profile=xxx라는 식으로 호출하면 해당 properties의 설정들을 가져올 수 있다.

#### 3️⃣ application.properties에 코드 추가
```
spring.profiles.include=oauth
```

#### 4️⃣ .gitignore에 추가하기
```
application-oauth.properties
```

💔 gitignore 설정이 안된다!
[.gitignore가 작동하지 않을 때](https://jojoldu.tistory.com/307)
- 캐시의 문제.. terminal에서 캐시 삭제!
```
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

#### 5️⃣ 구글 로그인 연동하기

User.java
```
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;
```

✳ @Enumerated(EnumType.STRING)
  - JPA는 DB로 저장할 때 Enum 값을 어떤 형태로 저장할지 결정한다.
  - 기본값이 int로 된 숫자. 여기서는 String으로 구분해주자
  - 각 사용자의 권한을 관리할 Enum 클래스

✳ 스프링 시큐리티에서는 권한 코드에 항상 *ROLE_*이 앞에 있어야 한다.

- User의 CRUD를 책임질 UserRepository.java

#### 6️⃣ 스프링 시큐리티 설정

1. build.gradle 의존성 추가
```
implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
```

2. OAuth 라이브러리를 이용한 소셜 로그인 설정 코드 작성
  - config.auth 패키지 생성. 앞으로 시큐리티 관련 클래스는 모두 이곳에

### SecurityConfig.java
```
@RequiredArgsConstructor
@EnableWebSecurity // Spring Security 설정들을 활성화 시켜준다.
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Override
    protected void configure(HttpSecurity http) throws Exception{
        http
                .csrf().disable()
                .headers().frameOptions().disable() // h2-console 화면을 이용하기 위해서는 해당 옵션을 disable 해야한다.
                .and()
                    .authorizeRequests()
                    .antMatchers("/", "/css/**", "/images/**", "/js/**", "/h2-console/**", "/profile").permitAll()
                    .antMatchers("/api/v1/**").hasRole(Role.USER.name())
                    .anyRequest().authenticated()
                .and()
                    .logout()
                        .logoutSuccessUrl("/")
                .and()
                    .oauth2Login()
                        .userInfoEndpoint()
                            .userService(customOAuth2UserService);
    }
}
```

✳ authorizeRequests
  - URL별 권한 관리를 설정하는 옵션의 시작점. authorizeRequests가 선언되어야만 antMatchers 옵션 사용 가능.

✳ antMatchers
  - 권한 관리 대상을 지정하는 옵션
  - URL, HTTP 메소드별로 관리가 가능
  - "/"등 지정된 URL 들은 permitAll() 옵션을 통해 전체 열람 권한을 줌
  - "/api/v1/**" 주소를 가진 API는 USER 권한을 가진 사람만 가능

✳ anyRequest
  - 설정된 값들 이외 나머지가 URL을 나타납니다.
  - 여기서는 authenticated를 추가해 나머지 URL들은 모두 인증돈 사용자들(로그인한 사용자)에게만 어훃아도록

✳ .logout().logoutSuccessUrl("/")
  - 로그아웃 기능에 대한 여러 설정의 진입점

✳ oauth2Login
  - OAuth 2 로그인 기능에 대한 여러 설정의 진입점


✳ userInfoEndpoint
  - OAuth 2 로그인 성공 이후 사용자 정보를 가져올 때의 설정을 담당한다.

✳ userService
  - 소셜 로그인 성공 시 후속 조치를 진행할 userService 인터페이스와 구현체 등록
  - 리소스 서버(소셜 서버스)들에서 사용자 정보를 가져온 상태에서 추가로 진행하고자 하는 기능 명시

### CustomOAuth2UserService.java

```
@RequiredArgsConstructor
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {

    private final UserRepository userRepository;
    private final HttpSession httpSession;

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        OAuth2UserService<OAuth2UserRequest, OAuth2User> delegate = new DefaultOAuth2UserService();
        OAuth2User oAuth2User = delegate.loadUser(userRequest);

        String registrationId = userRequest.getClientRegistration().getRegistrationId();
        String userNameAttributeName = userRequest.getClientRegistration().getProviderDetails()
                .getUserInfoEndpoint().getUserNameAttributeName();

        OAuthAttributes attributes = OAuthAttributes.of(registrationId, userNameAttributeName, oAuth2User.getAttributes());

        User user = saveOrUpdate(attributes);
        httpSession.setAttribute("user", new SessionUser(user));

        return new DefaultOAuth2User(
                Collections.singleton(new SimpleGrantedAuthority(user.getRoleKey())),
                attributes.getAttributes(),
                attributes.getNameAttributeKey());
    }


    private User saveOrUpdate(OAuthAttributes attributes) {
        User user = userRepository.findByEmail(attributes.getEmail())
                .map(entity -> entity.update(attributes.getName(), attributes.getPicture()))
                .orElse(attributes.toEntity());

        return userRepository.save(user);
    }
}
```

✳ registrationId
  - 현재 로그인 진행 중인 서비스를 구분하는 코드
  - 현재는 구글만 사용하지만 네이버도 연동 시 네이버인지 구글인지 구분

✳ userNameAttributeName
  - OAuth2 로그인 진행 시 키가 되는 필드 값. PK 같은 의미.
  - 구글의 기본 코드는 "sub"
  - 네이버, 카카오는 기본 지원 하지 않음

✳ OAuthAttributes
  - OAuth2UserService를 통해 가져온 OAuth2User의 attribute를 담은 클래스

✳ SessionUser
  - 세션에 사용자 정보를 저장하기 위한 Dto 클래스
  - 왜 User 클래스를 쓰지 않을까?

### OAuthAttributes.java
```
public class OAuthAttributes {

    private Map<String, Object> attributes;
    private String nameAttributeKey;
    private String name;
    private String email;
    private String picture;

    @Builder
    public OAuthAttributes(Map<String, Object> attributes, String nameAttributeKey, String name, String email, String picture) {
        this.attributes = attributes;
        this.nameAttributeKey = nameAttributeKey;
        this.name = name;
        this.email = email;
        this.picture = picture;
    }

    public static OAuthAttributes of(String registrationId, String userNameAttributeName, Map<String, Object> attributes) {
        if("naver".equals(registrationId)) {
            return ofNaver("id", attributes);
        }

        return ofGoogle(userNameAttributeName, attributes);
    }

    private static OAuthAttributes ofGoogle(String userNameAttributeName, Map<String, Object> attributes) {
        return OAuthAttributes.builder()
                .name((String) attributes.get("name"))
                .email((String) attributes.get("email"))
                .picture((String) attributes.get("picture"))
                .attributes(attributes)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    private static OAuthAttributes ofNaver(String userNameAttributeName, Map<String, Object> attributes) {
        Map<String, Object> response = (Map<String, Object>) attributes.get("response");

        return OAuthAttributes.builder()
                .name((String) response.get("name"))
                .email((String) response.get("email"))
                .picture((String) response.get("profile_image"))
                .attributes(response)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    public User toEntity() {
        return User.builder()
                .name(name)
                .email(email)
                .picture(picture)
                .role(Role.GUEST)
                .build();
    }
}
```

✳ of()
  - OAuth2User에서 반환하는 사용자 정보는 Map이기 때문에 값 하나하나를 변환해야 한다.

✳ toEntity()
  - User 엔티티 생성
  - OAuthAttributes에서 엔티티를 생성하는 시점은 처음 가입할 때
  - 가입시에는 기본 권한 GUEST

### SessionUser.java

```
public class SessionUser implements Serializable {
    private String name;
    private String email;
    private String picture;

    public SessionUser(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.picture = user.getPicture();
    }
}
```
- sessionUser에는 인증된 사용자 정보만 필요로 한다. 그 외에 정보는 없다.

☑ 왜 User 클래스를 사용하지 않고 SessionUser을 만들었을까?
  - 세션에 저장하기엔 User 클래스에서 직렬화를 구현하지 않아 저장할 수 없다. 그러나 User 클래스는 엔티티이기 때문에 직렬화를 할 수 없다.
  - 왜냐면 직렬화 대상에 자식들이 포함되는데 엔티티는 다른 엔티티와 관계를 맺을 수 있음(oneToOne..)
  - 그래서 직렬화 기능을 가진 세션 Dto를 추가로 만드는게 유지보수에 좋다. 

### 화면에 userName 넘겨주기

indexController.java
```
    @GetMapping("/")
    public String index(Model model){
        model.addAttribute("posts", postsService.findAllDesc());
        SessionUser user = (SessionUser) httpSession.getAttribute("user");
        
        if(user != null){
            model.addAttribute("userName", user.getName());
        }
        return "index";
    }
```

✳ httpSession.getAttribute("user")
  - 앞서 작성된 CustomOAuth2UserService에서 로그인 성공 시 세션에 SessionUser가 저장됨
  - 즉, 로그인 성공시 값을 가져올 수 있다.


### 어노테이션 기반 개선하기

1. 세션값을 가져오는 부분이 반복될 수 있다. 
  메소드 인자로 세션값을 바로 받을 수 있도록 하자. 
  
- @LoginUser 어노테이션
```
@Target(ElementType.PARAMETER) 
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser { // 
}
```

✳ @Target(ElementType.PARAMETER)
  - 어노테이션의 생성 위치. 파라미터로 지정했으니 메소드의 파라미터로 선언된 객체에서만 사용 가능.

✳ @interface
  - 해당 파일을 어노테이션 클래스로 지정한다.

2. 해야 할 일 메소드로 변경
```
@RequiredArgsConstructor
@Component
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {

    private final HttpSession httpSession;

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
        boolean isUserClass = SessionUser.class.equals(parameter.getParameterType());
        return isLoginUserAnnotation && isUserClass;
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        return httpSession.getAttribute("user");
    }
```

✳ supportsParameter()
  - 컨트롤러 메서드의 특정 파라미터를 지원한다.
  - 파라미터에는 @LoginUser 어노테이션이 붙어 있고, 파라미터 클래스 타입이 SessionUser.class인 경우 true 반환

✳ resolveArgument()
  - 파라미터에 전달할 객체 생성
  - 세션에서 객체를 가져온다.

3. WebConfig.java
 ```
 @RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {

    private final LoginUserArgumentResolver loginUserArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
        argumentResolvers.add(loginUserArgumentResolver);
    }
}
 ````

4. indexController 수정하기
```
@RequiredArgsConstructor
@Controller
public class IndexController {

    private final PostsService postsService;
    private final HttpSession httpSession;

    @GetMapping("/")
    public String index(Model model, @LoginUser SessionUser user){
        model.addAttribute("posts", postsService.findAllDesc());

        if(user != null){
            model.addAttribute("loginUserName", user.getName());
        }
        return "index";
    }
```

✳ @LoginUser SessionUser user
  - 세션 정보를 가져오는 곳 변경
  - 어느 컨트롤러든지 @LoginUser만 자용하면 세션 정보를 가져올 수 있다.

### 세션 저장소로 DB 사용하기
- 세션은 내장 톰캣(WAS)의 메모리에 저장되기 때문에 애플리케이션 실행 시 항상 초기화 된다.
- 즉, 배포할 때마다 톰캣이 재시작 되는 것
- 또한 2대 이상의 서버에서 서비스하고 있다면 톰캣마다 세션 동기화 설정을 해야한다.

☑ 현업에서는 세션 저장소에 대해 3가지 중 한 가지를 선택한다
  1. 톰캣 세션 사용
    - 일반적으로 별다른 설정 하지 않음
    - 톰켓(WAS)에 세션이 저장되기 때문에 2대 이상의 WQS가 구동되는 환경에서는 톰캣들 간의 세션 공유를 위한 추가 설정이 필요
  2. MySQL과 같은 데이터베이스를 세션 저장소로 사용
    - 여러 WAS 간의 공용 세션을 사용할 수 있는 가장 쉬운 방법
    - 많은 설정이 필요 없지만, 결국 로긍니마다 DB IO가 발생해 성능상 이슈가 발생할 수 있음
    - 보통 로그인 요청이 많이 없는 백오피스, 사내 시스템 용도에서 사용
  3. Redis, Memcached와 같은 메모리 DB를 세션 저장소로 사용한다.
    - B2C 서비스에서 가장 많이 사용하는 방식
    - 실제 서비스로 사용하기 위해서는 Embedded Redis와 같은 방식이 아닌 외부 메모리 서버가 필요하다.
=> 여기서는 두번째 방식(DB를 세션 저장소로 사용)을 선택
  - 설정이 간단
  - 사용자가 많은 서비스가 아님
  - 비용 절감
  - 이후 AWS에서 이 서비스를 배포하고 운영할 때를 생각하면 Redis와 같은 메모리 DB를 사용하기는 부담스러움. 별도의 사용료를 지불해야 하기 때문이다.

1️⃣ 의존성 등록 

build.gradle
```
implementation 'org.springframework.session:spring-session-jdbc'sp
```
application.properties에 세션 저장소를 jdbc로 선택하도록 저장
```
spring.session.store-type=jdbc
```

=> db에 세션을 위한 테이블 spring_session, spring_session_attribbutes가 생성된다.

<img src="https://user-images.githubusercontent.com/60870438/171438524-e8df3d80-a1d0-449a-9afe-a0c353c993f1.png" width=50%>


### 네이버 로그인

[네이버 오픈 API](https://developers.naver.com/apps/#/register?api=nvlogin)

#### 1️⃣ application-oauth.properties 추가
```
# registration
spring.security.oauth2.client.registration.naver.client-id={클라이언트 ID}
spring.security.oauth2.client.registration.naver.client-secret={클라이언트 비밀번호}
spring.security.oauth2.client.registration.naver.redirect-uri={baseUrl}/{action}/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.naver.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.naver.scope=name,email,profile_image
spring.security.oauth2.client.registration.naver.client-name=Naver

# provider
spring.security.oauth2.client.provider.naver.authorization-uri=https://nid.naver.com/oauth2.0/authorize
spring.security.oauth2.client.provider.naver.token-uri=https://nid.naver.com/oauth2.0/token
spring.security.oauth2.client.provider.naver.user-info-uri=https://openapi.naver.com/v1/nid/me
spring.security.oauth2.client.provider.naver.user-name-attribute=response #(1)
```
☑ (1)
  - 기준이되는 user_name의 이름을 네이버에서는 response로 해야한다.
  - 이유는 네이버의 회원 조회 시 반환되는 JSON 형태 때문
  - 스프링 시큐리티에선 하위 필드를 명시할 수 없다. 최상위 필드만 user-name으로 지정이 가능하다. 네이버의 응답값 최상위 필드는 resultCode, message, response

#### 2️⃣ 스프링 시큐리티 설정 등록

OAuthAttributes.java에 네이버인지 판단하는 코드만 추가하면 된다.


### 기존 테스트에 시큐리티 적용하기

- 기존에는 바로 API를 호출할 수 있었으나 시큐리티 옵션이 활성화되면서 인증된 사용자만 API를 호출할 수 있다.
- 테스트 코드마다 인증한 사용자가 호출한 것처럼 작동하도록 수정해야 한다.

#### 전체 테스트 수행하기

Gradle > Tasks > verification > test 클릭!

<img src="https://user-images.githubusercontent.com/60870438/171443798-e30a690a-77af-4b83-a979-23637547d3ac.png" width=50%>

[junit5 수정](https://jojoldu.tistory.com/539)
> 휴, 책에 있는 내용이 옛날 버전이라 이상하게 안되더니 새로 올라와있었습니당..
> 
> 그리고 원래 테스트 방법대로 안돼서 프로젝트 > test > Run All 'Tests'로 실행시켰더니 잘 된다.

1. 어노테이션 변셩
2. db 변경

#### 임의로 인증된 사용자 추가하기
@Test에 @WithModckUser(rols="USER")추가하기

✳ @WithModckUser(roles="USER")
  - 인증된 모의 사용자를 만들어 사용한다.
  - roles에 권한을 추가한다.
  - 즉, 어노테이션으로 인해 ROLE_USER 권한을 가진 사용자가 API를 요청하는 것과 동일한 효과를 가지게 된다.
  - 하지만 이 어노테이션은 MockMvc에서만 작동한다. 우리는 @SpringBootTest로 해 놓았으니 이를 MockMvc로 사용하도록 변경하자

