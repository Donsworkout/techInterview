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
트랜잭션의 모든 연산들은 수행 완료되거나 아니면 아예 안한 상태를 보장해야 한ㄷ.

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

## 6. 인덱스 

## 7. Slow Query 대처 

## 8. Query Optimization

## 9. NoSQL vs RDBMS

## 10. MongoDB