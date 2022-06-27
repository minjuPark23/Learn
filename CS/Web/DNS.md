# DNS(Domain Name System)

```
DNS는 TCP/IP 네트워크에서 사용되는 네임 서비스의 구조.
우리가 쓰는 영문/한글 주소를 IP 네트워크에서 찾아갈 수 있도록 IP로 변경.
```

### DNS를 운영하는 '네임 서버'

<img src="https://user-images.githubusercontent.com/60870438/175900402-00840f54-d3c5-4f3e-bea5-38f69f0a3367.png" width=70%>

1. PC 브라우저에 www.naver.com 입력 -> PC는 미리 설정된 local DNS에게 'www.naver.com'으로 된 hostname에 대한 IP 주소를 요청
2. local DNS에 'www.naver.com'이 캐싱되어 있지 않을 경우, 다른 DNS 서버와 통신을 시작해 IP 주소를 찾는다.
  - 먼저 Root DNS 서버에게 IP 주소 질의. 이때 local DNS가 Root DNS에 질의하기 위해 각 local DNS는 Root DNS 서버의 정보(IP Address)가 미리 설정되어 있음.
3. Root가 모를 경우 '.com' 도메인을 사용하므로 local DNS는 com DNS를 관리하는 TLD DNS 서버의 정보를 포함해 응답한다.
4. 받은 IP 주소를 가지고 com Domain을 관리하는 DNS 서버에게 IP 주소 질의
5. com Domain을 관리하는 DNS 서버 또한 해당 정보가 없음. Local DNS 서버에게 "naver.com" 도메인을 관리하는 DNS 서버의 정보를 포함해 응답
6. Local DNS 서버는 "naver.com" 도메인을 관리하는 DNS 서버에게 가서 다시 질의.
7. 해당 DNS 서버는 naver.com 도메인을 직접 매니징하므로 해당 IP 주소를 반환할 수 있음
  - 이를 수신한 Local DNS는 www.naver.com에 대한 IP 주소를 캐싱하고 PC에게 전달.

▶️ 이렇게 Local DNS 서버가 여러 DNS를 차례대로(Root -> com -> naver.com DNS) 거치는 것을 Recursive Query라고 한다.


