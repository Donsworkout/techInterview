[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. JDK / JRE / JVM

![jdk](https://user-images.githubusercontent.com/26560119/58600070-f0afe580-82bd-11e9-8c55-be299736b11b.jpg)


#### JDK (개발킷, Java Development Kit) 
- JRE + 개발을 위해 필요한 도구 ex) javac (컴파일러), java

#### JRE (실행용, Java Runtime Environment)
- JRE (실행용) 는 자바 런타임 환경으로 하드웨어 위에 위치하는 계층이며  
운영 체제에 상관없이 컴파일된 자바 프로그램을 실행될 수 있도록 돕는다. 즉 기반 OS를 추상화 해준다.

> cf) RUNTIME 환경  
컴퓨터 내에서 프로그램이 기동되면, 그것이 바로 그 프로그램의 런타임이다. (컴파일 다음단계)

#### JVM (Java Virtual Machine)

![266F3C445805EF0A31](https://user-images.githubusercontent.com/26560119/58601083-3c648e00-82c2-11e9-808a-cd6537dfa07d.png)


- JIT(Just In Time)으로 컴파일된 class 파일은 bytecode(중간언어) 로 만들어 지는데, bytecode를 OS에 맞는 기계어로 변환해주는 역할 (c++ 은 빌드때 바로 기계어로 감)

- JVM에 있는 클래스로더가 런타임때 바이트코드를 받아 자바 인터프리터로 기계어로 해석하고 실행한다.

- 이래서 자바가 OS 독립적이라고 하는 것이다. 

- JVM 의 구조에 대해 좀 더 살펴 보자면, 아래 그림 참고 고고

<img width="826" alt="스크린샷 2019-05-30 오전 10 11 18" src="https://user-images.githubusercontent.com/26560119/58601331-4cc93880-82c3-11e9-86f5-49c420d82392.png">

1) class loader 가 빌드된 자바 목적파일 (class) 을 메모리로 로드한다.

2) Execution Engine이 적재된 바이트 코드를 읽어 기계어로 변환하는데, 처음에는 interpreter을 이용하여 한줄 씩 명령어를 실행하다가, 나중에는 JIT 컴파일러를 이용한다.

> JIT 컴파일러는 적절한 시간에 전체 바이트코드를 기계어로 번역하고, 바이트 코드를 캐싱하여 뒤에 다시 같은 명령을 컴파일 하지 않도록 하여 성능을 높힌다.

3) **Garbage Collector (GC)**  

![ㅇㅇㅇㅇ](https://user-images.githubusercontent.com/26560119/58604123-35dc1380-82ce-11e9-8c51-927b767a0365.png)

- Runtume Data Area 의 Heap 메모리 영역에서 참조되지 않는 객체들을 탐색 후 제거한다.

- Heap의 young 영역 (eden, survivor1/2) 에서 일어나는 Minor Gc가 있고,  

- Heap의 old 영역 (old. permanent) 에서 일어나는 Major GC가 있다.

- 메소드 영역에는 클래스, static 변수 등이 저장되는데, 이때 올라온 클래스에서 인스턴스를 생성하면 Heap 영역에 인스턴스가 생성되는 것이다.

- 스택 메모리에는 지역변수, 파라미터, 리턴 값 등이 저장된다.

#### GC 절차
#### Minor GC
1) 최초 객체 생성시 Eden 영역으로, Eden이 꽉차면 Survivor1 영역에 그대로 복사된다.  
이때 참조하고 있지 않은 객체는 삭제된다. 
> 메모리 복사 되는 작업이 Minor GC 이다. 

2) Eden 도 꽉 차고 survivor 1 영역도 꽉 차게 되면, 참조 검사를 하고 참조 되고 있는 객체만 survivor 2 영역에 복사한다.

3) 위 과정 중 일정 횟수이상 참조되고 있는 객체들을 survivor 2에서 Old 영역으로 복사한다.
> survivor 영역 중 하나는 비어 있어야 한다. 

4. 위 과정을 계속 반복하며, survivor 2 영역까지 차기 전에 Old 로 비워준다. 

> Minor GC 만으로도 heap 유지가 가능하도록 유도하는 것이 좋다.

#### Major GC

1. Old 영역 모든 객체들을 참조되고 있는 지 검사하고

2. 참조되지 않는 객체들을 모아 한꺼번에 제거한다 (모든 쓰레드 중지)

3. Heap 메모리 중간중간 구멍을 없애기 위해 재구성을 하기도하는데, 옮기는 도중에 다른 쓰레드가 메모리 참조하지 못하도록 모든 쓰레드를 정지시킨다.

=> 결론 : GC를 잘 모니터링 해야 한다. (stop the world 조심)


## 2. Java EE / SE / ME 차이
### 1) 자바 SE (Standard Edition)
데스크톱, 서버, 임베디드를 위한 표준 자바 플랫폼, SE가 다른 에디션의 기반이다.  
JDBC뜽 기본적인 기능이 들어가 있음 

### 2) 자바 EE (Enterprise Edition)  
자바를 이용한 서버측 개발을 위한 플랫폼 SE에 장애복구 및 분산 멀티미디어를 제공하여 대규모, 신뢰성 등을 보장한다.

### 3) 자바 ME (Micro Edition)  
폰, 셋톰박스 등에서 java 이용할 수 있도록 만들어진 플랫폼 


## 3. 자바 Array / List
### Array
> 일반적으로 배열은 크기가 확정되어 있고 인덱스가 중요할 때, 효율이 좋아 많이 쓰인다.
- 인덱스를 활용하여 빠른 조회가 가능하다.
- 물리적으로 인접해 있어 cache hit (공간 지역성) 의 가능성이 높아진다.
- 대신 리스트에 비해 기능이 없다.
- 가변 배열을 쓰면 기존 배열 그대로 두고 새로운 길이로 배열 할당 후 데이터 복사하고 기존 배열을 삭제한다.
- 요소 삭제시 메모리가 낭비된다.

### List 
> 인덱스가 중요하지 않을떄 빈틈없음 데이터의 적재가 가능
- 순서가 있는 데이터의 모임
- 배열의 index가 그 배열의 Key 라면, 리스트의 index 는 순서라는 의미 정도이다.
- 데이터 추가 / 삭제가 많이 일어난다면 리스트가 적합하다.

### 자바의 list / 배열  
- 자바는 배열과 리스트를 모두 지원한다.

~~~java
// 배열 - 추가, 삭제가 어렵다. 직접 구현해야한다.
int[] numbers = {10,20,30,40,50};

// 리스트 (ArrayList)
ArrayList numbers = new ArrayList();

numbers.add(10); // 추가
numbers.remove(0); // 삭제

// 리스트 (LinkedList)
LinkedList numbers = new LinkedList();

numbers.add(10); // 추가
numbers.remove(0); // 삭제
~~~

1) ArrayList  
- 자바의 배열을 이용해서 만든 리스트로, 인덱스를 사용해서 값을 가지고 올떄 매우 빠름
- 단 추가 / 삭제는 한칸씩 뒤로 밀고 떙기고 하는 과정때매 비교적 느림

2) LinkedList
- 추가 / 삭제가 빠르다. 
- 읽는것은 어레이 리스트 보다 느리다. (처음부터 순차 탐색 해야하기 떄문)


## 4. Java 의 장단점
### 장점
- 운영체제에 독립적이다 (JVM 있음)
- 객체지향 언어이다. (추상화 ,캡슐화, 상속, 다형성)
- 자동으로 메모리 관리 해 준다 (JVM GC)
- 멀티스레딩 API 가 있어 쉽게 구현할 수 있다.
- 동적 로딩을 지원한다. 고치면 해당 클래스만 재컴파일

### 단점 
- 조금 느리다. (c++, c에 비해 느리단 말임, 당연히 portability 와 Trade-off)

## 5. DAO / DTO

### 1) DAO
- DAO는 Persistence Layer(DB에 data를 CRUD하는 계층)이다.
- JPA로 따지면 QuestionRepository 같은거 
- MyBatis 같은 경우 Connection 을 여닫는 과정이 있음 
~~~java 
@Repository
public interface DailyStaticRepository extends JpaRepository<DailyStatic, Long> {
    Optional<DailyStatic> findByMerchantIdAndTradeDate(Merchant merchant, Date tradeDate);
    List<DailyStatic> findByTradeDate(Date tradedate);
}
~~~

### 2) DTO (Data Transfer Object)
- 읽기 전용 객체는 VO라고 부르기도 한다.
- 데이터 전송용 객체 
- 계층간 데이터 교환을 위한 자바빈즈 
- 계층은 Controller, View, Service, Persistence 계층을 말한다.