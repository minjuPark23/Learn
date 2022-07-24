## Socket

```
- socket 연결은 TCP/IP 프로토콜을 기반으로 맺어진 네트워크 연결 방식
- socket 연결 방식으로 프로그래밍 하는 것을 소켓 프로그래밍이라고 한다.
```

#### 💡 소켓 프로그래밍

```
Server와 Client가 특정 Port를 통해 실시간으로 양방향 통신을 하는 방식
```

#### 특징

- 계속 연결을 유지하는 연결지향형 방식이기 때문에 실시간 통신이 필요한 경우 자주 사용된다. (ex. 채팅)
- 실시간 동영상 스트리밍의 경우 스트리밍하는 사람이 방송을 종료할 때까지 서버에 요청을 해야하는데 이러한 구조는 서버 부하를 발생한다.
  따라서 계속 요청하는 것이 아닌 1번의 연결을 통해 연결 유지


## Web Socket

#### 💡 웹 소켓

```
웹 페이지의 한계에서 벗어나 실시간으로 상호작용하는 웹 서비스를 만드는 표준 기술.(비트코인, 주식)
```

- HTTP 프로토콜은 클라이언트에서 서버로의 단방향 통신을 위해 만들어짐
- 실시간 웹을 구현하기 위해서 양방향 통신이 필요한데, web Socket 이전에는 Polling, Streaming 방식의 Ajax 코드를 이용해 이를 구현.
- 하지만 이전의 방식은 각 브라우저마다 구현 방식이 달라 어려움.
- 이를 위해 HTML5 표준의 일부로 Web Socket이 등장!

#### ✅ 일반 TCP Socket과 Web Socket의 차이점

- 일반 HTTP Request를 통해 Handshaking 과정을 거쳐 최초 접속이 이루어 진다.
- 어플리케이션 레이어에서 Handshaking이 일어난다는 의미

#### 특징

- 소켓을 통해 자유롭게 데이터 통신 가능
- 기존의 요청-응답 관계 방식보다 쉽게 데이터 교환
- 다른 HTTP Request와 마찬가지로 80 포트를 통해 웹 서버에 연결한다.
- http:// 대신 ws:// 으로 시작하며 Streaming과 유사한 방식으로 푸시
- client인 브라우저와 마찬가지로 Web Server도 WebSocket 기능을 지원해야 한다. 
- Web Server: ex. Jetty, Node.js 등
- 브라우저(WebSocket 지원): Chrome, Safari, Firefox 등. 각종 모바일 브라우저도 O
- WebSocket 프로토콜은 아직 확정되지 않아 브라우저 별로 지원하는 WebSocket 버전이 다름.

#### 장점

- HTTP Request를 그대로 사용하기 때문에 기존의 80, 443로 접속하므로 추가로 방화벽 설정하지 않고 양방향 통신 가능
- HTTP 규격인 CORS 적용이나 인증 등의 과정을 기존과 동일하게 사용


## 💡 Socket.io

<img src="https://user-images.githubusercontent.com/60870438/180655856-07af6064-8b7d-4b25-816f-fcf5985ba782.png" width=70%>

```
각 클라이언트(브라우저) 호환되는 기술을 사용해주는 자바스크립트 모듈
```

- 다양한 방식의 실시간 웹 기술을 손쉽게 사용할 수 있는 모듈 (웹 클라이언트로의 푸시 시원)
- WebSocket, FlashSocket, AJAX Long Polling, AJAX Multi part Streaming, IFrame, JSON Polling 등 다양한 방법을 하나의 API로 추상화한 것
- 즉, Socket.io는 JavaScript를 이용해 브라우저 종류에 상관없이 실시간 웹을 구현할 수 있도록 한 기술

#### 특징

- Socket.io는 현재 바로 사용할 수 있는 기술
- WebSocket 프로토콜은 IETF에 권장하는 표준 프로토콜으로 WebSocket을 지원하지 않는 브라우저는 
  브라우저의 모델, 버전에 따라 AJAX Long Polling, MultiPart Streaming, IFrame을 이용한 푸시, JSON Polling, Flash Socket 등 다양한 방법으로 내부적으로 푸시 메시지를 보낸다.
- 즉, WebSocket을 지원하지 않는 어느 브라우저라도 일관된 모듈로 메시지 전송


## 💡 OSI 6 계층

PDU(Protocol Data Unit)

<img src="https://user-images.githubusercontent.com/60870438/180657897-4ae743eb-cf63-489e-9c66-dec9f273d958.png">

#### 프로토콜 단위

애플리케이션, 표현, 세션 - data(Message)

전송 - segment

네트워크 - packet

데이터 링크 - frame

물리 - bit

#### 데이터의 캡슐화

<img src="https://user-images.githubusercontent.com/60870438/180658058-bb7f3813-8cbe-49fe-8b63-5b8eced0c718.png" width=70%>

```
PDU(Protocol Data Unit)는 SDU(Service Data Unit)과 PCI(Protocol Control Information)으로 구성.
SDU는 전송하려는 데이터, PCI 제어 정보
PCI에는 송/수신자 주소, 오류 검출 코드, 프로토콜 제어 정보 존재
데이터 + 제어 정보 -> 캡슐화(Capsulation)
즉, 캡슐화란 네트워크를 통과하기 위해 전송하는 데이터를 감싸고 네트워크 통과 이후 다시 벗겨내 전송하는 기능
```




## 참고
[socket.io와 webSocket](https://theheydaze.tistory.com/565)
[신입 웹 개발자 면접 질문](https://minchoi0912.tistory.com/93)
