1. 정규형 중 완전 함수 종속을 추구하는 곳 제일 첫번째 것은 
  - 제 1 정규형
  - 제 2 정규형 ✅
  - 제 3 정규형
  - BCNF

```
제 1 정규형: 속성 값이 모두 원자값이어야 한다.
제 2 정규형: 기본키가 아닌 속성은 기본키에 완전 함수 종속
제 3 정규형: 기본키가 아닌 속성이 다른 속성을 결정할 수 없다. 이행적 함수 종속성
BCNF: 모든 결정자가 항상 후보키가 되어야 한다.
```

2. 트랜잭션의 특성이 아닌 것은
  - 일관성
  - 가용성 ✅
  - 고립성
  - 지속성

```
트랜잭션이란?
작업의 완전성을 보장해주는 트랜잭션.
논리적인 작업을 모두 완벽하게 처리하지 못할 경우 원 상태로 복구해 일부만 적용되는 현상을 막는다.

특성 ACID
- 원자성(Atomicity): 트랜잭션의 연산은 데이터베이스에 모두 반영되든지 아니면 전혀 반영되지 않아야 한다.
- 일관성(Consistency): 시스템이 가지고 있는 고정요소는 트랜잭션 수행 전과 트랜잭션 수행 완료 후의 상태가 같아야 한다.
- 고립성(Isolation): 각각의 트랜잭션은 서로 독립적으로 수행되어야 한다.
- 지속성(Durability): 트랜잭션이 정상적으로 종료된 다음에는 영구적으로 데이터베이스에 결과가 저장되어야 한다.
```

3. CORS의 HTTPHeader 중 접근 가능한 url을 설정하는 것은
- Access-Control-Allo-Method
- Access-Control-Allo-Credentials
- Access-Control-Allo-Headers
- Access-Control-Allo-Origin ✅

```
다른 도메인으로부터 리소스가 요청될 경우 리소스는 cross-origin HTTP 요청에 의해 요청된다.
CORS는 타 도메인 간에 자원들 공유할 수 있게 해준다.
Cross-Origin Resource Sharing 표준은 브라우저가 사용하는 정보를 읽을 수 있도록 허가된 집합을 서버에게 알려주어 허용하는 것.
```

4. CPU 스케줄러 중 선점형 스케줄러인 것은
- FCFS
- SRTF ✅
- SJF

```
CPU 스케줄러의 대상은 Ready Queue에 있는 프로세스들

1. First Come First Served
  - 먼저 온 순서대로 처리한다. 비선점형 스케줄링
  - 소요시간이 긴 프로세스가 먼저 시작되면 효율성이 낮아진다.
2. Shortest-Job-First
  - 다른 프로세스가 먼저 도착해도 CPU burst time이 짧은 프로세스에게 선 할당된다. 비선점형 스케줄링
  - 사용 시간이 긴 프로세스는 영원히 CPU를 할당받지 못할 수도 있다.
3. Shortest Remaining Time First
  - 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다. 선점형 스케줄링
  - 현재 수행 중인 프로세스의 남은 burst time이 더 짧은 프로세스가 도착하면 CPU를 뺏긴다.
4. Priority Scheduling
  - 우선순위가 높은 프로세스에게 CPU 할당.
  - 선점형: 더 높은 우선순위를 가진 프로세스가 도착하면 실행중인 프로세스를 멈추고 CPU 다시 선점
  - 비선점형: 더 높은 우선순위를 가진 프로세스가 도착하면 Ready Queue의 Head에 넣는다.
5. Round Robin
  - 각 프로세스가 동일한 할당 시간을 갖는다. 할당 시간이 지나면 프로세스는 선점당하고 Ready Queue 뒤로 가서 다시 줄을 선다.
  - 프로세스의 context를 save 할 수 있기 때문에 가능하다.
```

5. Spring MVC에서 클라이언트의 요청을 받아 처리하고 그 결과를 디스패처 서블릿에게 전달하는 컴포넌트는
- Dispatcher Servlet
- Heandler Mapping
- Controller ✅
- ViewResolver

```

Dispatcher Servlet: 클라이언트의 요청을 먼저 받아들이는 서블릿으로, 요청에 맞는 컨트롤러에게 요청을 전달
Handler Mapping: 해당 요청이 어떤 컨트롤러에게 온 요청인지 검사
Controller: 클라이언트의 요청을 받아 처리하여 결과를 디스패처 서블릿에게 전달
ViewResolver: View의 이름을 통해 알맞은 View를 찾아
View: 사용자에게 보여질 UI 화면
```

6. 함수형 프로그래밍의 장점이 아닌 것은
- 순수 함수
- 데이터의 불변성
- 부수 효과가 큼 ✅

7. 객체지향의 특징 4가지 중 재사용성을 위한 특징은
- 추상화
- 캡슐화
- 상속 ✅
- 다형성

8. Java에서 Garbage 발생 시 GC의 내용 중 옳은 것은
- Minor GC와 Middle GC, Major GC가 존재한다.
- 스택 영역에서 활동한다.
- Major GC는 Old Generation(old 영역)에서 동작한다.✅
- c언어에서는 freeze() 통해 메모리를 해제한다.

```
Garbage Collection이란
JVM에서 불필요한 메모리를 Garbage Collector가 정리해준다.
JVM의 Heap 영역은 처음 설계될 때 두가지의 전제를 갖는다.
  - 대부분의 객체는 금방 Unreachable(접근 불가능)한 상태가 된다.
  - 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.
  - 즉, 객체는 대부분 일회성이다.

생존 기간에 따라 Heap영역을 Young, Old 두 영역으로 나눈다.
  - Young 영역: 새롭게 생성된 객체 할당. 
    금방 Unreachable 상태가 되므로 잘 사라진다.
    Minor GC라고 부른다.
  - Old 영역: Young 영역에서 Reachable 상태를 유지해 살아남은 객체가 복사된다.
    Major GC라고 부른다.
```

9. JPA의 영속성 컨텍스트의 이점이 아닌 것은
- 2차 캐시 ✅
- 동일성 보장
- 쓰기 지연
- 변경감지

```
JPA의 영속성 컨텍스트의 이점 5갸지
  - 1차 캐시: 조회가 가능하며 1차 캐시에 없으면 DB에서 조회하여 1차 캐시에 올려 놓습니다.
  - 동일성 보장: 동일성 비교가 가능합니다.(==)
  - 쓰기 지연: 트랜잭션을 지원하는 쓰기 지연이 가능하며 트랜잭션 커밋하기 전까지 SQL을 바로 보내지 않고 모아서 보낼 수 있습니다.
  - 변경 감지(Dirty checking): 스냅샷을 1차 캐시에 들어온 데이터를 찍습니다. commit 되는 시점에 Entity와 스냅샷과 비교하여 update SQL을 생성합니다.
  - 지연 로딩: 엔티티에서 해당 엔티티를 불러올 때 그 때 SQL을 날려 해당 데이터를 가져옵니다.
```
