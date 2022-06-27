#### Domain이란?

```
웹 브라우저를 통해 특정 사이트에 진입할 때, IP 주소 대신해 사용하는 주소
```

# DNS(Domain Name System)

```
DNS는 TCP/IP 네트워크에서 사용되는 네임 서비스의 구조.
우리가 쓰는 영문/한글 주소를 IP 네트워크에서 찾아갈 수 있도록 IP로 변경.(매칭)

Domain name <--> IP 주소 변환이 가능하도록 개발된 데이터베이스 시스템이다.
```

### DNS를 운영하는 '네임 서버'

#### 1. DNS의 원리

```
기존에는 IP주소와 도메인 주소 변경에 따라 hosts 파일을 관리해야 했다면, DNS Server에서 변경된 내용을 관리해주기 때문에 효율적이다.
```

#### 2. DNS 이름 구조와 등록 과정

EX) www.naver.com

```
www: 호스트 명으로 서브 도메인 (sub domain)
naver: 도메인 명 (secend-level domain/Authoritative DNS 서버)
.com: 최상위 도메인 (Top-level domain)
.: Root domain
```

- 계층 구조는 root > top-level > second-level > sub domain인 순
- 각 요소는 인접해있는 하위 계층 요소의 정보(IP 주소 등)을 알고있으며, 인접하지 않은 요소에 대해서는 정보를 가지고 있지 않다.

#### 3. 도메인 접근 과정

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

💡 계층적으로 보면 이렇다.

<img src="https://user-images.githubusercontent.com/60870438/175942706-b58b1654-2466-4958-9392-b566b22133be.png" width=70%>

1. example.com 요청 > "." Root NS에 접근해 top-level 정보 제공
2. ".com" Top-level NS에 접근해 NS 정보 제공
3. example = NS에 접근해 IP 정보 제공
4. IP 정보를 얻은 host의 DNS Server는 해당 주소로 접속 시도


#### 4. DNS 서버 종류

- Root DNS Server: TLD DNS 서버 IP들을 저장해두고 안내하는 역할.


- TLD(최상위 도메인) DNS Server: 도메인 등록 기관(Registry)이 관리하는 서버, Authoritative DNS 서버 주소를 저장해두고 안내하는 역할. 
  어떤 도메인 묶음이 어떤 Authoritative DNS Server에 속하는지 아는 이유는 도메인 판매 업체(Registrar)의 DNS 설정이 변경되면 도메인 등록 기관(Registry)으로 전달이 되기 때문임.


- Authoritative DNS Server: 실제 개인 도메인과 IP 주소의 관계가 기록/저장/변경되는 서버. 그래서 권한의 의미인 Authoritative가 붙음. 일반적으로 도메인/호스팅 업체의 ‘네임서버’를 말하지만, 개인 DNS 서버 구축을 한 경우에도 여기에 해당함.


- Recursive DNS Server: 인터넷 사용자가 가장 먼저 접근하는 DNS 서버.
  한 번 거친 후 얻은 데이터를 일정 기간(TTL/Time to Live) 동안 캐시라는 형태로 저장해 두는 서버. 
  직접 도메인과 IP 주소의 관계를 기록/저장/변경하지는 않고 캐시만을 보관하기 때문에, Authoritative와 비교되는 의미로 반복의 Recursive가 붙음. 
  
  
🎈재귀 쿼리 - 재귀 쿼리에서는, 확인자가 레코드를 찾을 수 없는 경우, DNS 클라이언트는 DNS 서버(일반적으로 DNS 재귀 확인자)가, 요청한 자원 레코드 또는 오류 메시지를 사용하여 클라이언트에 응답하도록 요구합니다.

🎈반복 쿼리 - 이 경우, DNS 클라이언트는 DNS 서버가 가능한 최상의 응답을 반환하도록 합니다. 쿼리한 DNS 서버가 쿼리 이름과 일치하는 이름을 갖고 있지 않은 경우, 하위 수준의 도메인 네임스페이스에 대해 권한 있는 DNS 서버에 대한 참조를 반환합니다. 그러면 DNS 클라이언트가 참조 주소를 쿼리합니다. 이 프로세스는 오류 또는 제한 시간 초과가 발생할 때까지 추가 DNS 서버가 쿼리 체인을 중단한 상태로 계속됩니다.


[참고]
[DNS 동작 과정](https://velog.io/@sms8377/TIL23-DNS-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95)
