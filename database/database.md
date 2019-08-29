[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. 데이터베이스 풀 (pool)  
- 어플리케이션 스레드에서 DB Transaction 을 위해서는 DB connection이 필요하다.
- Connection pool은 미리 Connection 을 여러개 만들어 놓고 스레드에게 빌려준다.  
- Thread는 Connection을 사용하고 사용 후 Pool 에 반환한다. 
- Connection이 없다면 대기하고, 가다렸다가 Connection 객체를 받는다.

![db-connection-02](https://user-images.githubusercontent.com/26560119/58722096-8f992680-8411-11e9-9276-5837b9e207f8.png)


#### 장점
- 매 연결마다 Connection 객체 생성 소멸 안해도 된다.
- 그러므로 DB 접근 시간이 단축된다.
- Connection 수를 제한하여 메모리와 DB 부하를 줄일 수 있다.

> Thread Pool 도 있다. Thread 수가 Connection 보다 많은게 좋다고 한다.  
모든 스레드가 디비 커넥션을 할 것이 아니기 때문.  
스레드를 많이 사용할수록 당연히 메모리는 많이 먹는다.

## 2. 트랜잭션 (Transaction)
> 데이터베이스의 상태를 변화시키는 하나의 논리적인 작업 단위 (로직 단위)  

- Commit : 트랜잭션이 완전히 끝남, 데이터베이스가 일관된 상태로 돌아옴
- Rollback : 트랜잭션 중 결함이 발생하면 트랜잭션 전체를 취소함 (Undo)

### 트랜잭션의 성질 (ACID)
1) 원자성 (Atomicity) ALL OR NOTING  
트랜잭션의 모든 연산들은 수행 완료되거나 아니면 아예 안한 상태를 보장해야 한다.

2) 일관성 (Consistency)  
트랜잭션 완료 후에도 DB는 일관된 상태를 유지해야 한다

3) 독립성 (Isolation)  
하나의 트랜잭션이 실행하는 도중 변경한 데이터는, 해당 트랜잭션이 완료되기 전까지 다른 트랜잭션이 참조하지 못한다.

4. 지속성 (Durability)  
성공적으로 수행된 트랜잭션은 영원히 반영된다.

### 트랜잭션의 상태 
![transaction-status](https://user-images.githubusercontent.com/26560119/58741705-3ce25d80-8457-11e9-8989-de963c642cce.png)

- Commit / Aborted (undo) 상태가 DB가 보장하는 상태 

### Isolation Level
1. Read Uncommitted  
커밋도 안했는데, 바뀐 값을 읽어올 수 있다.

> 발생 문제점 : Dirty Read, Non-Repeatable Read, Phantom Read

2. Read Committed  
커밋이 완료된 데이타만 읽을 수 있다. 커밋 되기 전에 읽으면 예전 데이터로 읽고, 커밋 후면 바뀐 데이터로 읽어온다. 
Dirty Read가 발생할 여지는 없으나, Read Uncommitted 수준보다 동시 처리 성능은 떨어진다.  
> 발생 문제점 : Non-Repeatable Read, Phantom Read

3. Repeatable Read  
트랜잭션이 완료되기 전까지는 처음느낌 그대로 ㄱ (다른 트랜잭션이 수정 불가)

> 발생 문제점 : Phantom Read


4. Serializable  
Strict 2PL 같은거 

### 격리성 관련 문제점

1. Dirty Read
커밋도 안된거 읽어서 롤백되면 당황하는 거 

2. Non-Repeatable Read (리드하고 있는데 남이 와서 커밋해버림)  
트랜잭션 내에서 다시 읽었는데 값이 바뀌어있음 (수정 / 삭제)

3. Phantom Read  
없던 데이터가 생겨있음 (삽입) 트랜잭션 도중 새로운 레코드가 삽입될 수 있어서 그럼

## 3. 스키마 란?
- 데이터베이스 구조 / 제약조건에 관해 전반적으로 명시해 놓은 것 

![191456384EA616D008](https://user-images.githubusercontent.com/26560119/58744543-57303180-847f-11e9-885f-9cb844acd51c.jpeg)

- 외부 스키마는 OFFSET 처럼 물리적 위치까지 나와있는 것.

## 4. 조인
- 아래 링크 너무 굳  
https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#join


## 5. 파티셔닝 
- 내가 쓴 링크 
https://github.com/Donsworkout/cs_wiki/blob/master/database_system/22_a_parallel_dbms.md
- 남이 쓴 링크  
https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#%ED%8C%8C%ED%8B%B0%EC%85%94%EB%8B%9D
### 5-1) 샤딩 심화  
- CUBRID SHARD, GIZZARD 등은 샤딩 플랫폼이다.
- 샤딩은 수평 파티셔닝이다.
- JOIN, 일관성 등에 약하다.
- Hibernate Shard 는 어플리케이션 서버 샤딩
- MongoDB는 데이터베이스 자체에서 샤딩 기능을 내장한다.
- Cubrid, Gizzard 는 미들웨어 샤딩 플랫폼
- 데이터베이스 위치 (site) 는 추상화 되어 있다.
- 당연히 물리적 샤딩 하면, 네트워크 IO 발생 
- 미들웨어는 DB와 웹 어플리케이션의 중간 

### 5-2) 샤딩의 단점, 주의사항

- 두 개 이상의 샤드(site) 에 대한 JOIN 연산을 할 수 없다.  
- auto increment (serial) 등은 샤드별로 달라질 수 있다. (인스타그램)  
- last_insert_id() 값은 유효하지 않다.  
- shard key column 값은 update하면 안 된다(delete - insert 사용).  
- 하나의 트랜잭션에서 **두 개 이상** 의 샤드에 접근할 수 없다.
- Scale up 을 항상 고려하자 
- 해시샤딩은 나중에 스케일업 시 해시함수 교체 비용 많이 든다
- **샤드키를 잘 정하자** RANGE PARTITIONING 이면, 그 범위를 잘 정할것, 랜덤키도 가능 

### 5-3) 샤딩 참고
- 논리적 샤딩도 가능 
- UUID(universally unique identifier) 가 중요

## 6. 인덱스 
> 사용 이유 : READ FAST (검색 > 변경)  

- 인덱스 엔트리는 서치키로 레코드 서브셋을 찾고, 맨 아래 리프 노드에는 rid가 있다.

### 6.1. 데이터 엔트리의 정보 (리프 노드)
ALTERNATIVE 1 :  
search key k를 포함하는 실제 데이터 레코드 (인덱스 파일 구성)  
heap, sorted 파일 구조 대신 쓰기도 한다.

ALTERNATIVE 2 :  
<k, rid of data> :   
유일키일 경우 하나의 rid Primary, Unique index 라고 부른다

ALTERNATIVE 3 :  
<k, list of rids of data>:   
유일키가 아닐경우 rid list Secondary index 라고 부른다.

### 6.2. 인덱스를 이용할때 최적의 조건  
- 테이블당 하나만 생성할 수 있는 clustered 인덱스가 가장 빠르긴 함 (실제 인접)
- unclustered 도 rid 찾는것은 빠르지만, range query 일때 여러 페이지에서 레코드를 메모리로 올려야 해서 I/O cost 가 크다.
- 풀스캔은 차라리 인덱스 안쓰는게 낫다. (왜냐 인덱스 스캔에도 불리하고, 분포도가 10% 넘어가면 disk IO가 너무 커짐)
- B + tree 라면 fanout 을 높이는게 포인트, 따라서 인덱스 sKey 압축하기도 함
- 따라서 복합 서치키도 너무 길면 fanout 줄여서 트리 업데이트가 더 자주 일어남   
- COUNT MIN AVG 등 직계함수는 INDEX ONLY 도 가능하다.
- 풀스캔이 왜 더 빠를때가 있냐 하면, 인덱스는 버킷 찾으면 따로따로 디스크 블락을 불러 무수히 많은 IO가 생김 , 근데 절대량도 중요 3000건 미만일시!
- 풀스캔은 멀티블락IO를 한다. 한꺼번에 블락 끌어옴! 

### 6.3. **Query workload**
- 어떤 relation 이 쿼리에 참여했는지
- 어떤 attr을 검색했는지
- 어떤 attr이 select 나 join 연산에 참여했고, 해당 연산이 얼마나 selective 한지  
- 어떤 업데이트 연산을 사용했고, 어떤 attr에 영향을 미치는지

### 6.4. 어떤 인덱스를 쓸 것인가 
https://github.com/Donsworkout/cs_wiki/blob/master/database_system/8_overview_of_storage_and_indexing.md#7-%EC%96%B4%EB%96%A4-%EC%9D%B8%EB%8D%B1%EC%8A%A4%EB%A5%BC-%EC%93%B8%EC%A7%80-%EC%84%A0%ED%83%9D%ED%95%98%EA%B8%B0\

## 7. Slow Query 대처 
> 슬로우 쿼리 로그는 시간이 오래 걸렸던 쿼리를 로그로 저장해 놓은 것. FULL SCAN 이나 잘못된 인덱스 사용으로 발생하기도 한다.
### 7.1. Slow Query 원인  
- 쿼리 실행 계획 체크하여 확인, 인덱스 잘못 쓴 경우가 많음 
- 서치키 or 복합서치키 prefix에 where 절 처음 조건이 오지는 않는지 체크
- 버퍼 풀 사이즈가 너무 작지는 않은지 
- 어쨋든 로그 분석을 잘하자 

## 8. Query Optimization
일단은 평소에 DB에 관련된 최적화가 이루어져있는지 체크하는 과정이 중요하다. 가장 간단하게 할 수 있는 Query 최적화가 있습니다.

그리고 평소에 Query 실행계획을 보면서 Query를 적용하기 전에 테스트하는 과정이 필요합니다.

https://github.com/Donsworkout/cs_wiki/blob/master/database_system/12_overview_of_query_evaluation.md

### 8.1. 쿼리 평가 
> 쿼리 옵티마이저는 차선의 선택을 한다. 메타데이터 업데이트가 실시간은 아니기 때문 

### 8.2. 쿼리 최적화의 interests 
1) 주어진 Query에서 어떤 plan 들이 고려되어야 하는가?
2) 각 Plan의 Cost는 얼마인지 ?

### 8.3. 쿼리 최적화 과정 
1. Access Path 선택 
- full scan or index! 
- prefix 조건을 확인해 보세요!

2. 관계연산을 위한 알고리즘 선택  
- Selection 
  - RF 10% 100 페이지 10000 튜플일때, 클러스터는 100 IO 지만.  
  unclustered 일때는 10000 IO 발생가능, FULL SCAN 요망 
  - 이런식으로 selection 어떻게 할지 생각함 
  - unclustered 라면 RF 5% 이상은 풀스캔이 낫다.

- Projection
  - 여기서 가장 비싼 연산은 DISTINCT 
  - 소팅이나 해시 기반으로 중복을 제거한다.

- Join
  - https://github.com/Donsworkout/cs_wiki/blob/master/database_system/12_overview_of_query_evaluation.md#3-3-join
  - 인덱스를 이용하여 조인하면 더 빠름
  - 둘이 다 비교 안해도 조인칼럼 인덱스를 통해 튜플 찾으면 되니깐 ..
  - left deep plan 이 파이프라이닝을 이용하여 더 빠름 


## 9. NoSQL - 주로 MongoDB vs RDBMS
### 9.1 MongoDB 의 특장점
- Schema less (데이터의 유연한 변화)
- Read, Write 성능 뛰어남
- RDBMS의 한계, Scale Out 구조를 채택 
- **Scale out 시 AutoSharding을 지원한다.**
- Json 구조, 가독성
- WiredTiger storage 엔진은 트랜잭션 ACID 보장
- JSON으로 바로 넣을 수 있다.
- **삽입 부하가 큰 곳에 적절**
- 위치 기반 데이터에 좋음 (Geospatial)

### 9.1.1 쓰면 좋을 상황
- 데이터량이 많이 확장을 고려해야 하거나
- RW 퍼포먼스가 좋아야 하거나 
- 요구사항에 따라 스키마가 변할 가능성이 많을때 


### 9.2 MongoDB 단점 
- Document 자체 용량이 row 보다 큼 (3배가량) 
- 따라서 메모리 버퍼에도 올라가는 document가 적어 Disk I/O가 커진다.
- Clustered Index 가 없다
- ACID 등 안정성이 RDBMS보다 떨어짐 

### 9.3 MongoDB 의 지형 인덱스와 쿼리
- 위치 기반 기능이 존재한다.
- MongoDB는 geosparial query를 지원한다.
- Geospatial Indexes 를 지원한다.

**지형 인덱스를 사용한 billRun**  
https://enzine.tistory.com/m/entry/MongoDB%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0-%EB%95%8C

### 실제 사례 연구: 결제 시스템  
- MongoDB를 데이터 저장소로 사용한 오픈소스 결제 솔루션인 BillRun

- BillRun 시스템은 매월 5억건 이상의 통화 기록을 처리하는 이스라엘에서 가장 빠르게 성장하는 통신망 기업에서 사용되었다. 

- 스키마가 없는 설계 덕분에 새로운 통화 기록 유형을 시스템에 빠르게 도입할 수 있었다. 

- 새 필드 추가를 제한하거나 데이터 증가를 제한하지 않고도 BillRun은 이미 실무에서 테이블당 수 TB에 달하는 데이터를 다루었다.

- 샤딩을 통해 예산을 다 쓰지 않고도 선형적, 수적인 증가가 가능했다.

- 초당 2천건 이상의 통화 기록을 삽입할 때처럼 MongoDB 아키텍쳐는 삽입 부하가 높은 시스템에 적합하다. 

- 위치 기반 기능을 활용해 사용자의 사용 패턴을 분석하고 어디에 통신망 인프라를 투자할 것인지 정했다.