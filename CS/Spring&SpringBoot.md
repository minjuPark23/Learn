# Spring과 Spring Boot

```
Spring?
개발자들의 겨울이 끝났다!
```

```
Spring Book makes it easy 쉽게!
to create stand-alone 단독적인
production-grade 상용화 수준의
spring based Applications 스프링 기반 애플리케이션
that you can just run
```

## Spring

### 1. dependency(의존성)

모든 dependency를 버전까지 적어줘야한다.

=> 짧아짐. 버전 관리도 권장 버전으로 자동 설정


### 2. configuration

=> config 파일을 만들지 않아도 된다.

=> application.properties, application.yml로 간단하게

EX) thymeleaf

![image](https://user-images.githubusercontent.com/60870438/183136301-d39b4992-0307-4280-b14e-18c9abc5986e.png)

이거만 하면
![image](https://user-images.githubusercontent.com/60870438/183136217-a1fe3c91-9f1d-46d0-bb26-72e5a66205d0.png)

안에 이미 적혀있다.
![image](https://user-images.githubusercontent.com/60870438/183136597-c29a2158-a1a9-4fde-97e7-ced5e0bf30ba.png)

### 3. embedded server

내장 서버. tomcat => 서버 구동 시간이 절반 가까이 단축

build해서 jar 파일로 간단하게 배포할 수 있다.

-> java -jar $REPOSITORY/$JAR_NAME &
내장 서블릿 컨테이너 덕분에 jar 파일로 간단 배포!

### 💡 정리

1. 간편한 설정
2. 편리한 의존성 관리 & 자동 권장 버전 관리
3. 내장 서버로 인해 간단한 배포 서버 구축
4. 스프링 Security, Data JPA 등의 다른 스프링 프레임워크 요소 쉽게 사용
