## [DataBase] DB

1. 데이터 베이스: 데이터의 저장소
2. SQL: 구조화된 질의 언어로 관계형 데이터베이스에서 사용되는 언어

### DBMS(Database Management System)

데이터베이스 관리 시스템으로 데이터베이스를 관리하고 운영하는 소프트웨어

데이터베이스에 여러명의 사용자나 응용 프로그램과 공유하고 동시 접근이 가능하게 한다.

<img src="https://user-images.githubusercontent.com/60870438/173314484-1c11d593-db19-4b54-a02f-3735a4fa4918.png" width=70%>


#### DBMS 언어 종류

- DDL(Data Definition Language, 데이터 정의어)
  - 데이터베이스 스키마를 정의할 때 사용하는 언어
  - 데이터베이스 구축 및 수정을 목적으로 사용하는 언어
  - CREATE, DROP, ALTER, CREATE INDEX

- DML(Data Management Language, 데이터 조작어)
  - 사용자가 데이터를 처리할 때 사용하는 언어
  - 데이터베이스의 데이터를 실질적으로 조작하는 언어
  - SELECT, UPDATE, INSERT

- DCL(Data Control Language, 데이터 제어어)
  - 데이터를 제어하기 위한 언어
  - 데이터베이스 트랜잭션을 명시하고 권한 부여 혹은 취소
  - 주로 데이터의 보안, 무결성, 병행 수행 제어를 정의하는 언어
  - COMMIT, ROLLBACK, GRANT, REVOKE

#### DBMS의 장점
- 데이터 중복 및 불일치 최소화
- 데이터의 표준화
- 데이터의 일관성 및 무결성 유지
- 동시접근 가능

#### DBMS의 분류
- 계층형, 망형, 관계형(Relational), 객체지향형  등이 있고. 나는 주로 관계형 DBMS를 사용했다.


### RDBMS(Relational Database Management System)

관계형 데이터베이스 관리 시스템으로 관계형 데이터 베이스를 수정하고 관리할 수 있는 소프트웨어

테이블(table)이라는 최소 단위로 구성되며 테이블은 하나 이상의 열(column)과 행(row)를 갖는다.

#### SQL(Structured Query Language)

관계형 데이터베이스에서 사용되는 언어

SQL은 특정 회사에서 만드는 것이 아니라 국제 표준화 기구에서 SQL에 대한 표준을 정해 발표한다.

그러나 SQL을 사용하는 DBMS를 만드는 회사에 따라 각 회사의 제품 특성을 반영한 SQL을 사용한다.

<img src="https://user-images.githubusercontent.com/60870438/173315601-84bc0e5b-14ff-480d-8307-2003056a7b56.png" width=70%>

### DB

#### 정규화

- 제1정규형
  - 모든 속성 값이 원자값을 갖도록 분해한다.

<img src="https://user-images.githubusercontent.com/60870438/173316943-ab1bae04-50ac-4fe6-812a-3d20b91063bb.png" width=70%>
- 고객번호로 주소를 알 수 있다,
- 주문번호로 고객번호를 알 수 있다,

- 제2정규형
  - 제1정규형을 만족하고 기본키가 아닌 속성이 기본키에 완전 함수 종속되도록 분해한다.

<img src="https://user-images.githubusercontent.com/60870438/173318145-aebca110-0e3f-4415-b5e1-46e1a2771d11.png" width=70%>
