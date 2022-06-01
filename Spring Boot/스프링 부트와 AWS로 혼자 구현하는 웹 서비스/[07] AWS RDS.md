### AWS에 데이터베이스 환경을 만들어보자

- AWS에서는 관리형 서비스인 RDS를 제공한다.
- RDS란? AWS에서 지원하는 클라우드 기반 관계형 데이터베이스

우리는 MariaDB를 사용한다.
  - 가격
  - Amazon Aurora 교체 용이성

MariaDB는 MySQL을 기반을 ㅗ만들어짐
  - 동일 하드웨어 사양으로 MySQL보다 향상된 성능
  - 활성화된 커뮤니티
  - 다양한 기능
  - 다양한 스토리지 엔진
  - "MySQL에서 MariaDB로 마이그레이션 해야 할 10가지 이유" 검색

퍼블릭 엑세서 가능 -> 예
  - 이후 보안 그룹에서 지정된 IP만 접근하도록 할 예정


#### 설정

파라미터 그룹 > 파라미터 편집 > 
- time_zone > Asia/Seoul
- char > 모두 utf8mb4
- collation > 모두 utf8m4_general_ci 

utf8과 utf8mb4의 차이는 이모지 저장 가능 여부

- max_connections > 150

#### 연결

1. 데이터베이스 > 선택 > 파라미터(보안) 그룹 연결

2. 보안그룹 인바운드 추가

#### 인텔리제이 연결

Plugins > database Navigator 
