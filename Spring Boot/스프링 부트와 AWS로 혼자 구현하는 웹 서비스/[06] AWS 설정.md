## AWS EC2

#### 클라우드 형태

1. Infrastructure as a Sercice(Iaas)
  - 기존 물리 장비를 미들웨어와 함께 묶어둔 추상화 서비스
  - 가상머신, 스토리지, 네트워크, 운영체제 등의 IT 인프라를 대여해 주는 서비스
  - ex. AWS의 EC2, S3

2. Platform as a Service(PaaS)
  - Iaas를 한 번 더 추상화한 서비스로 많은 기능이 자동화되어 있다.
  - ex. AWS의 빈스톡, 헤로쿠

3. Software as a Service(SaaS)
  - 소프트웨어 서비스
  - ex. 구글 드라이브, 드랍박스, 와텝 등

우리는 여러 클라우드 서비스(AWS, Azure, GCP 등) 중 AWS 선택
  - 첫 가입시 1년간 대부분 섭비스 무료
  - 클라우드에서 기본적으로 지원하는 기능(모니터링, 로그관리, 백업, 복구, 클러스터링 등)이 마낳아 개인이나 소규모일 때 개발에 더 집중할 수 있음.
  - 많은 기업이 AWS 사용
  - 사용자가 많아 국내 자료와 커뮤니티 활성화

> 우리는 AWS 서비스의 IaaS를 사용할 예정

#### EC2란?
  - AWS에서 제공하는 성능, 용량 등을 유동적으로 사용할 수 있는 서버.

아마존 리눅스 1 지원 종료 -> 2 사용

아마존 리눅스 2는 센토스 7 버전 자료를 그대로 사용할 수 있다.
> 굳이 아마존 리눅스 AMI를 사용하는 이유는?
  - 아바존이 개발해서 지원받기 쉬움
  - 레드햇 베이스로 레드햇 계열의 배포판을 많이 다뤄본 사람일 수록 쉽게 사용
  - AWS의 각종 서비스와 상성 좋음
  - Amazon 독자적인 개발 리포지터리를 사용해 yum이 빠름
  
#### 1️⃣ puttygen에서 
  - pem -> ppk 변환
  
#### 2️⃣ putty.exe
  - Session > HostName ec2-user@{탄력적 IP}
  - Connection > SSH > AUTH > ppk연결

#### 3️⃣ 완료!

![image](https://user-images.githubusercontent.com/60870438/171477520-a710f4b5-e825-4740-a31e-3498fe6d1b3a.png)

### 아마존 리눅스 서버 생성시 설정

#### 1️⃣ java 8 설치
```
sudo yum install -y java-1.8.0-openjdk-devel.x86_64 # 설치
sudo /usr/sbin/alternatives --config java # 인스턴스의 버전 변경

java -version # 자바 버전 확인
```

#### 2️⃣ 타임존 변경
```
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

date # 시간 확인
```

#### 3️⃣ 호스트네임 변경
```
sudo vim /etc/sysconfig/network

sudo reboot # 재부팅
```
HOSTNAME={변경할 호스트네임}
저장후 재부팅
[호스트네임 변경](https://soobarkbar.tistory.com/226)

#### 4️⃣ 호스트네임 추가

```
sudo vim /etc/hosts
```
127.0.0.1 {등록한 호스트네임} <- 추가하기

- 등록 확인
```
curl {등록한 호스트 네임}
```


