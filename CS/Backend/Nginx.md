# Why Nginx

### Nginx vs apache server

```
Nginx란?
Nginx는 웹서버로 리버스 프록시, 로드밸런서, http cache로 쓰일 수 있는 소프트웨어
요청에 응답하기 위해 이벤트 기반 구조 채택
=> 현재 웹서버의 1등!
```

#### 🎈 질문

- 이벤트 기반 구조란?
- apache Server도 웹서버인데 차이점은?


## Apache Server

- 1995년도
- request가 들어오면 커넥션을 위해 프로세스 생성
- 즉, 새로운 클라이언트의 요청마다 프로세스 생성 (== unix의 os 네트워크 커넥션 모델 형성 과정)
- 그런데 프로세스를 새로 생성하는 건 시간이 오래 걸린다.
