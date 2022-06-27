#### 1. DNS 서비스란?
```
호스트의 도메인 네임을 IP로 변환해주는 서비스를 말하며 
DNS 서버는 계층 구조로 수현된 분산 데이터 베이스의 구성 요소로 Root, Top leve Domain(TLD), Authoitative, Local DNS Server가 존재
```

#### 2. DNS 서비스 과정을 설명하세요.(캐싱이 없는 경우)
```
- 호스트가 Local DNS 서버에게 도메인(www.naver.com)으로 IP 주소 요청
- local DNS 서버는 Root DNS 서버에게 도메인 네임 요청
- Root DNS 서버는 com을 인식하고 TLD 서버의 주소를 local에게 전송
- local DNS 서버는 TLD 서버에게 받은 Domain Name을 보내고 TLD 서버는 naver.com을 인식한 후 Authoritative 서버의 주소를 전송
- local DNS는 Authoritative 서버에게 Domain Name을 보내고 www.naver.com의 IP 주소를 얻어온다
- 변환된 IP 주소를 호스트가 넘겨받고 이를 통해 어플리케이션 통신!
```
