스프링 시큐리티(Spring Security)는 막강한 인증(Authentication)과 인가/권한 부여(Authorization)의 기능을 가진 프레임워크

### 스프링 시큐리티와 스피링 시큐리이 Oauth2

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
  - 설정된 값들 이외 나머지가 URL에 나타납니다.

✳
