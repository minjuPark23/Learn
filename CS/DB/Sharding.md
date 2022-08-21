# 샤딩(Sharding)

```
Shard: 조각 또는 파편
Sharding(샤딩): 한 테이블의 row를 여러개의 서로 다른 테이블, 즉 파티션으로 물리적 분류
                Horizontal Partitioning
```

<img src="https://user-images.githubusercontent.com/60870438/185793741-4a6a2336-9ad7-4853-9293-879f67b9242b.png" width=80%>

- 전체 데이터베이스 하나의 테이블에 전부 들어가기 힘든 데이터가 등장해 DBMS가 테이블을 관리하기 힘들어짐에 따라 적용한다.
- 기존에 하나로 구성될 스키마를 복제해 구성, 각각의 샤드에 어떤 데이터가 저장될지 샤드키를 기준으로 분리한다.
- 주로 Application Level에서 실행되며 어떤 샤드로 읽기와 쓰기를 전송할지 정의하는 코드를 갖는다.
- DB에 내장된 샤딩 기능이 있어 Database Level에 실행되기도 함.

### ✅ 장점

- Scale-Out 가능
- 스캔 범위를 줄여 쿼리 반응 속도 증가
- 장애가 샤드 단위로 발생

### ✅ 단점

- 프로그래밍 복잡도 증가. 가능하다면 샤딩을 피하는게 좋다.
- 데이터가 한쪽 샤드로 몰릴경우(Hotspot), 샤딩의 무의미
- 잘못 사용할 경우 risk가 큼
- 한번 샤딩 사용시, 샤딩 이전의 구조로 돌아가기 힘듬

#### ✔️ 샤딩 피하기

- DB 서버의 Scale-Up
- Read 부하가 클 경우 Cache 사용 밑 DB Replication
- Table 일부 컬럼만 주로 사용할 경우 Vertical Partitioning

</br>
### 샤딩의 방식

- DB 분산 방식
- 분산된 데이터 읽는 법
- Shard Key 정하기
- = 샤드를 균일하게 하는 것이 목적

#### 1. Hash Sharding

<img src="https://user-images.githubusercontent.com/60870438/185796142-77ec4a0b-b2d4-4816-9fb4-4907ab56d46e.png" width=70%>

- Shard Key: DB의 Id를 hashing하여 결정

- 한번 Num을 정하면 수정하지 못함
- 단순한 Hashing으로 데이터가 저장되는 공간적 효율에 대한 고려는 할 수 없다.

#### 2. Dynamic(Range) Sharding

<img src="https://user-images.githubusercontent.com/60870438/185795844-d61a5368-9bbd-402a-ab00-ddf122872e2f.png" width=70%>

- 일종의 Locator Service를 통해 샤드 키 정하기
- Locator Service를 한번 거쳐 샤드키를 얻기 때문에 확장에 유연하다. -> cluster가 포함하는 Node의 개수를 늘리면 된다

- Data Relocation하게 되면 Locator Service의 Shard Key Table을 일치시켜 줘야 한다.
- Locator가 성능을 위해 Cache를 쓰거나 Replication하면 Locator 의존한다는 단점이 있다.


```
Hash와 Dynamic은 Key-Value 형태로 지원
Key-Value가 아닌 다양한 객체로 구성된다면?
```
 
 #### 3. Entity Group
 
<img src="https://user-images.githubusercontent.com/60870438/185798563-16cc131c-4f2c-4e99-a1d2-78062bb015a5.png" width=70%>

- RDBMS의 join, index, transaction을 사용해 Application의 복잡도를 줄이는 방식

- 하나의 물리적 Shard에 쿼리 진행해 효율적
- 하나의 Shard에 강한 응집도
- 데이터는 자연스럽게 사용자별로 분리
- 사용자가 늘어남에 따라 확장성이 좋은 Partitioning

- 

# 참고

- [Database-Shard](https://nesoy.github.io/articles/2018-05/Database-Shard)
