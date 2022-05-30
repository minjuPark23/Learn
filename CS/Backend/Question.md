### 1️⃣ 많은 트래픽이 발생한 경우 대처 방법

- 스케일 업(Scale Up): 서버에 CPU나 RAM 등을 추가해 서버의 하드웨어 스펙을 향상시키는 방법
- 스케일 아웃(Scale out): 서버를 여러 대 추가해 시스템을 증가시키는 방법

### 2️⃣ CORS란?

1. Cross-Origin-Resource-Sharing으로 도메인이 다른 2개의 사이트가 데이터를 주고 받을 때 발생하는 문제.
  - ex. domain1.com에서 domain2.com으로 데이터를 요청하면 cors 에러가 난다.

2. 발생 이유
  - 서버 내에서요청이 허락된 도메인에만 데이터를 주기 위해서
3. 해결 방법
  - Access-Control-Allow-Origin: {도메인}을 Response 헤더에 추가해야 한다.

4. 다양한 설정
  - Access-Control-Allow-Orgin : 요청을 보내는 페이지의 출처 [ \*, 도메인 ] 
  - Access-Control-Allow-Methods : 요청을 허용하는 메소드. Default : GET, POST
  - Access-Control-Max-Age : 클라이언트에서 preflight 요청 (서버의 응답 가능여부에 대한 확인) 결과를 저장할 시간
  - Access-Control-Allow-Headers : 요청을 허용하는 헤더

<img src="https://user-images.githubusercontent.com/60870438/170975370-445297a2-9487-4075-98e6-37adb81b9cb9.png" width=70%>

### 3️⃣ 아파치는 멀티 프로세스인가 멀티 쓰레드인가?

- 아파치는 기본적으로 멀티 프로세스로 구현되어있으나 설정에 따라 멀티 쓰레드를 같이 운용할 수 있다.


### 4️⃣ 톰캣은 멀티 프로세스인가 멀티 쓰레드인가?

- 톰캣은 요청을 처리하기 위한 쓰레드 풀을 관리한다.
- 요청이 오면 해달 쓰레드 풀에서 쓰레드를 꺼내 요청을 처리하게 한다.


### 5️⃣ Servlet이란?

- 서블릿이란 클라이언트의 요청을 처리하고 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술.
- Spring MVC에서 Controller로 이용되며, 사용자의 요청을 받아 처리한 후 결과를 반환


### 6️⃣ Spring 기초 지식

1. DI(Dependency Injection, 의존성 주입): 의존 대상(사용할 객체)을 주입을 통해 받는 방식
  - 객체 간의 의존 관계를 객체 자신이 아닌 외부 조립기가 수행
3. DL(Dependency Look-up): 의존 대상(사용할 객체)을 검색(look-up)을 통해 반환
4. Ioc(Inversion of Control, 제어의 역행): 직접 제어해야 하는 부분에 대한 권한을 프레임워크 등에 넘기는 기술
  - 계층간의 의존관계 결합도를 낮추기 위해 적용(loose coupling)
  - 낮은 결합도, 유지보수성 향상
5. AOP(Aspect Oriented Programming, 관점지향 프로그래밍): 공통의 관심 사항을 추출해 원하는 곳에 적용하는 기술

### 6️-1 AOP와 OOP

- OOP(Object Oriented Programming, 객체지향 프로그래밍)
- 


### 7️⃣ 디스패처 서블릿(Dispatcher Servlet)이란?

- 톰캣과 같은 서블릿 컨테이너를 통해 들어오는 모든 요청을 제일 먼저 받는 프론트 컨트롤러.
- 공통된 작업을 처리한 후, 적절한 세부 컨트롤러로 작업을 위임해준다.
- 세부 컨트롤러는 작업을 처리한 후 View를 Dispatcher Servlet에 넘긴다.

![image](https://user-images.githubusercontent.com/60870438/170977346-7977ce1a-bfc3-48f0-82c7-3dce9a80723e.png)

### 8️⃣ Spring에서 싱글톤 패턴


### 9️⃣ MVC 패턴이란? 

1. MVC(Model-View-Controller)패턴은 아키텍쳐를 설계하기 위한 디자인 패턴

2. 구성요소
  - Model: 데이터를 저장하는 컴포넌트
  - View: 사용자 인터페이스(UI) 컴포넌트
  - Controller: 사용자의 요청을 처리하고 Model과 View를 중개하는 컴포넌트

### 🔟 Spring MVC란?

1. Spring MVC란 웹 애플리케이션 개발을 위한 MVC 패턴 기반의 웹 프레임워크입니다. Spring MVC는 애플리케이션의 구성요소를 Model, View, Controller로 분리합니다. 또한 Spring MVC는 아래와 같은 컴포넌트들로 구성됩니다.

2. 구성요소
  - Dispatcher Servlet: 클라이언트의 요청을 먼저 받아들이는 서블릿으로, 요청에 맞는 컨트롤러에게 요청을 전달
  - Handler Mapping: 해당 요청이 어떤 컨트롤러에게 온 요청인지 검사
  - Controller: 클라이언트의 요청을 받아 처리하여 결과를 디스패처 서블릿에게 전달
  - ViewResolver: View의 이름을 통해 알맞은 View를 찾아 
  - View: 사용자에게 보여질 UI 화면

### 1️⃣ Spring MVC 작동원리

![image](https://user-images.githubusercontent.com/60870438/170978648-740267b1-1f2a-459e-80e7-c3e71ad5e883.png)

1. 클라이언트는 URL을 통해 요청을 전송한다.
2. 디스패처 서블릿은 핸들러 매핑을 통해 해당 요청이 어느 컨트롤러에게 온 요청인지 찾는다.
3. 디스패처 서블릿은 핸들러 어댑터에게 요청의 전달을 맡긴다.
4. 핸들러 어댑터는 해당 컨트롤러에 요청을 전달한다.
5. 컨트롤러는 비지니스 로직을 처리한 후에 반환할 뷰의 이름을 반환한다.
6. 디스패처 서블릿은 뷰 리졸버를 통해 반환할 뷰를 찾는다.
7. 디스패처 서블릿은 컨트롤러에서 뷰에 전달할 데이터를 추가한다.
8. 데이터가 추가된 뷰를 반환한다.    

### 2️⃣ Spring MVC와 Spring Boot

- 장점
  의존성 주입을 통해 컴포넌트 간의 결합도를 낮출 수 있어 단위테스트가 용이함
  제어의 역전을 통해 빈(객체)의 라이프싸이클에 관여하지 않고 개발에 집중할 수 있음
- 단점
  XML을 기반으로 하는 프로젝트 설정은 너무 많은 시간을 필요로 함
  톰캣과 같은 WAS를 별도로 설치해주어야 함

- 해결책(Spring Boot)
  자동설정(AutoConfiguration)을 도입하여 Dispatcher Servlet 등과 같은 설정 시간을 줄여줌
  프로젝트의 의존성을 독립적으로 선택하지 않고 spring-boot-starter로 모아두어 외부 도구들을 사용하기 편리함
  내장 톰캣을 제공하여 별도의 WAS를 필요로 하지 않음 


[참고]
[백엔드(Spring 위주)](https://mangkyu.tistory.com/95)
[백엔드 개발 기술 면접 정리](https://velog.io/@kk1112k/%EB%B0%B1%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C-%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-%EC%A0%95%EB%A6%AC-Spring-%EC%B6%94%EA%B0%80%EC%A4%91)
