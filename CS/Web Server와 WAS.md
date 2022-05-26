# [Network] 웹서버와 WAS

### 웹의 동작 원리

<img src="https://user-images.githubusercontent.com/60870438/167300519-c1cc5666-726c-4840-8de3-04320aedfafd.png" width=50%>

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

### Web Server와 Web Application Server의 차이

<img src="https://user-images.githubusercontent.com/60870438/170487445-8747063d-def0-4a20-b57c-f6fb976e4cbc.png" width=50%>

#### Web Server
- HTTP 프로토콜을 기반으로 하여 웹 브라우저의 요청을 서비스하는 기능 담당
- 정적인 컨텐츠(.html/ .png/ .jpg/ .css 등)을 제공할 때에는 WAS를 거치지 않고 바로 제공
- 동적인 컨텐프 요청이 들어왔을 때에는 해당 요청을 WAS에 보내고 처리한 결과를 반환 받는다.
- 종류: Apache Server, Nginx, IIS

#### Web Application Server
- DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 HTTP 통신을 통해 제공하는 기능 담당
- 웹 컨테이너 혹은 서블릿 컨테이너라고도 불린다. JSP, Servelt 구동 환경을 제공하는 서버
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
