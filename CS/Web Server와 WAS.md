# [Network] 웹서버와 WAS

### 1. 웹의 동작 원리

<img src="https://user-images.githubusercontent.com/60870438/167300519-c1cc5666-726c-4840-8de3-04320aedfafd.png" width=70%>

```
1, 2. 사용자가 웹 브라우저를 통해 찾고 싶은 웹 페이지의 주소 입력
3. 사용자가 입력한 URL 주소 중에서 도메인 네임(domain name) 부분을 DNS 서버에 검색
4. DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 프로토콜의 정보를 전달
5, 6. 웹 페이지 URL 정보와 전달받은 IP 주소는 HTTP 요청 메시지를 생성
  이렇게 생성된 HTTP 요청 메시지는 TCP 프로토콜을 사용해 인터넷을 거쳐 해당 IP 주소의 컴퓨터로 전송
7. 도착한 HTTP 요청 메시지는 HTTP 프로토콜을 사용해 웹 페이지 URL 정보로 변환
8. 웹 서버는 도착한 웹 페이지 URL 정보에 해당하는 데이터 검색
9, 10. 검색된 웹 페이지 데이터는 다시 HTTP 프로토콜을 사용해 HTTP 응답 메시지 생성
  이렇게 생성된 HTTP 응답 메시지는 TCP 프로토콜을 사용해 인터넷을 거쳐 원래 컴퓨터로 전송
11. 도착한 HTTP 응답 메시지는 HTTp 프로토콜을 사용해 웹 페이지 데이터로 변환
12. 변환된 데이터는 웹 브라우저에 의해 출력되어 사용자가 볼 수 있음
```

### 2. 정적 페이지와 동적 페이지

#### 정적 페이지(Static Pages)

<img src="https://user-images.githubusercontent.com/60870438/170493103-30f99e9b-e814-497e-bbdb-16dcefffc968.png" width=70%>

- Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환
- 항상 동일한 페이지를 반환한다
- 종류: image, html, css, js

#### 동적 페이지(Dynamic Pages)

<img src="https://user-images.githubusercontent.com/60870438/170493413-a861d5b9-8ba2-4324-a95f-adc503936032.png" width=70%>

- 전달받은 인자의 내용에 맞게 변경되는 contents를 반환
- 웹 서버에 의해서 실행되는 *프로그램* 을 통해서 만들어진 결과물.
  Servlet: WAS에서 돌아가는 Java Program
- 개발자는 Servlet에 doGet() 구현

### 3. Web Server와 Web Application Server의 차이

<img src="https://user-images.githubusercontent.com/60870438/170487445-8747063d-def0-4a20-b57c-f6fb976e4cbc.png" width=50%>

#### Web Server
- HTTP 프로토콜을 기반으로 하여 웹 브라우저의 요청을 서비스하는 기능 담당
- 정적인 컨텐츠(.html/ .png/ .jpg/ .css 등)을 제공할 때에는 WAS를 거치지 않고 바로 제공
- 동적인 컨텐프 요청이 들어왔을 때에는 해당 요청을 WAS에 보내고 처리한 결과를 반환 받아 클라이언트에게 응답
- 종류: Apache Server, Nginx, IIS(Windows 전용 Web Server)
- 단점
  - JSP나 PHP 같은 응용 프로그래밍 언어를 해석할 수 없다
  - 그래서 Java 기반 서버 사이드 언어를 처리할 수 있는 엔진 개발 -> WAS인 Tomcat

#### Web Application Server

<img src="https://user-images.githubusercontent.com/60870438/170494452-bafe6003-889d-4dae-84dc-ac34f2b61bf6.png" width=50%>

- DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 HTTP 통신을 통해 제공하는 기능 담당
- 웹 컨테이너(Web Container) 혹은 서블릿 컨테이너(Servlet Container)라고도 불린다. JSP, Servelt 구동 환경을 제공하는 서버
  Container이란? JSP, Servlet을 실행시킬 수 있는 소프트웨어
- 즉 WQS는 JSP와 Servlet의 구동 환경 
- 분산 트랜잭션, 보안, 메시징, 스레드 처리 등의 기능을 처리하는 분산 환경에서 사용
- 종류: Tomcat, JBoss, Jeus, Web Sphere
- 동작 프로세스
```
1. Web Server의 클라이언트 요청에 맞는 Servlet을 메모리에 올린다.
2. web.xml을 참조해 해당 Servlet에 대한 Thread 생성
3. HttpServletRequest와 HttpServletResponse 객체를 생성하고 그에 맞는 doGet, doPost 메소드를 호출해 생성된 동적 페이지를 Response 객체에 담아 WAS에 전달
  ex. doGet(HttpServletRequest req, HttpServletResponse rep)가 리턴하는 Response 객체를 WAS에 전달
4. WAS는 HttpResponse 형태로 바꾸어 WebServer에 전달하고 생성된 스레드와 HttpServletRequest, HttpServletResponse 객체를 제거한다.
```

> 예시: 레스토랑
> 주문을 확인하고 역할을 분배하는 메인 셰프는 Web Server. 그 아래에서 실제 요리하는 요리사들이 WAS
> 즉, AWS는 서버의 일을 돕는다.

#### Tomcat(톰캣) 이란?

<img src="https://user-images.githubusercontent.com/60870438/170494961-ed5628ca-dc6c-4205-ab87-62b27f8bbe43.png" width=50%>

- 동적 데이터 처리, DB와 연결되어 데이터 주고 받고, 프로그램으로 데이터 조작이 필요한 경우 사용.
- Apache Tomcat은 JSP 페이지의 실행 환경을 제공하는 WAS
- WAS로 자바코드를 이용해 HTML 페이지를 동적으로 생성해주는 프로그램
- 웹 서버와 웨 클라이언트를 결합함으로써 다양한 기능을 컨테이너에 구현해 다양한 역할을 수행할 수 있는 서버
- 클라이언트의 요청 -> 내부의 프로그램을 통해 결과를 먼둘면 다시 클라이언트에ㅔ
