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


- 컴파일된 class 파일은 bytecode(중간언어) 로 만들어 지는데, bytecode를 OS에 맞는 기계어로 변환해주는 역할 (c++ 은 빌드때 바로 기계어로 감)

- JVM에 있는 클래스로더가 런타임때 바이트코드를 받아 자바 인터프리터 / JIT 컴파일러로 기계어로 해석하고 실행한다.

- 이래서 자바가 OS 독립적이라고 하는 것이다. 

- JVM 의 구조에 대해 좀 더 살펴 보자면, 아래 그림 참고 고고

<img width="826" alt="스크린샷 2019-05-30 오전 10 11 18" src="https://user-images.githubusercontent.com/26560119/58601331-4cc93880-82c3-11e9-86f5-49c420d82392.png">

1) class loader 가 빌드된 자바 목적파일 (class / 바이트코드) 을 메모리로 로드한다.

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


## 2. Java 언어의 다양한 특징 
1. 자바는 변수를 사용하기 직전까지 변수 선인 시기를 늦추는 것을 좋은 방식으로 여긴다.

2. 상수는 final 키워드를 쓰며, 컴파일 타임에 할당되어 수정할 수 없다 (immutable)
    > 참고로 System.out.println 의 out은 System 클래스에 선언된 상수임
    ~~~java
    public static final PrintStream out
    ~~~

3. 

## 3. Java EE / SE / ME 차이
### 1) 자바 SE (Standard Edition)
데스크톱, 서버, 임베디드를 위한 표준 자바 플랫폼, SE가 다른 에디션의 기반이다.  
JDBC뜽 기본적인 기능이 들어가 있음 

### 2) 자바 EE (Enterprise Edition)  
자바를 이용한 서버측 개발을 위한 플랫폼 SE에 장애복구 및 분산 멀티미디어를 제공하여 대규모, 신뢰성 등을 보장한다.

### 3) 자바 ME (Micro Edition)  
폰, 셋톰박스 등에서 java 이용할 수 있도록 만들어진 플랫폼 

## 3. 자바의 Wrapper Class 
자바의 제너릭 클래스에는 불편한 제약이 하나 있는데,  
int, char 같은 기본 자료형을 타입 파라미터로 사용할 수 없다.    
(제너릭을 사용할때 타입 파라미터가 무조건 클래스이기 때문)  
이에 대한 해결책으로 Wrapper Class 를 사용한다.  
흔히 보는 Integer, Byte, Short, Long, Boolean 같은 것들이 이에 해당한다.

~~~java 
// ArrayList<int> <- 이런 형태는 절대 불가 
ArrayList<integer> numbers = new ArrayList<>();
~~~

기본 타입과 Wrapper 클래스 사이는 자동 형 변환이 가능하다.  
다른것은 별로 신경쓰지 않아도 되는데, Wrapper 클래스에서도 == 는 객체 참조를 비교한다는 사실을 잊지 말자 


## 4. 자바의 자료구조 
### Array
~~~java
String[] names = new String[100];
~~~ 
**new 연산자로 배열 생성 시**
- 숫자 타입의 배열은 0으로 채워진다 
- boolean 의 배열은 모두 false 로 채워진다.
- 객체 배열은 null 참조 

### Collection / Map
https://hackersstudy.tistory.com/26
https://onsil-thegreenhouse.github.io/programming/java/2018/02/18/java_tutorial_1-22/

![collectionsTable](https://user-images.githubusercontent.com/26560119/64118945-b3cf1f80-cdd3-11e9-8cfd-b8e5061a5ce6.png)

> 대표적으로는 사용 용도에 따라 List, Set, Map, Queue 으로 나눌 수 있다.

### 자료구조 인터페이스의 특징
> List, Set, Map, Queue 은 인터페이스이다.

1. List  
구현 클래스 : LinkedList, Stack, Vector  
간단 설명 : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.

2. Set   
구현 클래스 : HashSet, TreeSet  
간단 설명 : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다. 

3. Map  
구현 클래스 : HashMap, TreeMap, HashTable  
간단 설명 : 키(key)와 값(value)의 쌍으로 이루어진 데이터의 집합이다.  
순서는 유지되지 않고, 키는 중복을 허용하지 않으며 값의 중복을 허용한다.

4. Queue 

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

## 5. 자바 언어 기본
### 1. final 과 static final 의 차이
1. static
- 자바에서 static은 클래스 멤버를 설정하는 키워드이다.
- JVM 클래스 로딩 (.class -> Java Runtime) 시, 클래스의 모든 정보가 JVM class 영역에 올라가므로 static 키워드를 썼다면 이때 static 맴버가 적재된다.
- static 변수는 자바 메모리의 Method 영역으로 간다.
2. final 
- 상수, 메서드, 클래스에 사용가능하다.
- 상수라면 값 고정, 메서드라면 오버라이딩 금지, 클래스라면 상속 금지 

3. static final 
- 객체가 아니라 클래스에 존재하는 단 하나의 상수이다.
- 해당 변수를 클래스 변수로 만들면서 값을 고정시키겠다
    ~~~java
    private static final Logger LOGGER = LoggerFactory.getLogger(sth.class);

### 2. DataInputStream / DataOutputStream
https://hyeonstorage.tistory.com/239

### 3. throw 와 throws (아래 7번 참고)
- throw 는 **예외 처리** 이고, throws 는 **전가** 이다.

~~~java
public void getSth() throws Exception {
    if (stream.sthWrong())
        throw new IOException();
}
~~~
- 아래 처럼 썼을때 getSth 메서드를 호출한 쪽에서 예외를 전가받아 책임져야 한다
- throw에서 던질 수 있는 것은 Throwable 클래스를 상속받는 객체여야만 한다.

### 4. Reflection
> 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법  
동적으로 인스턴스를 생성한다.

[BeanFactory](http://wonwoo.ml/index.php/post/1571) 라는 Spring Container 는 어플리케이션이 실행한 후 객체가 호출 될 당시 객체의 인스턴스를 생성하게 되는데 그 때 필요한 기술이 Reflection 이다.

아래 링크에 자세히 나와 있다.  
https://gyrfalcon.tistory.com/entry/Java-Reflection

실제 리플렉션을 이용하여 보았다.  
필드 전체를 돌면서 각 필드의 어노테이션 값들에 접근하고,  
나아가 어떤 객체의 값을 설정하는 행위를 해 보았다.
~~~java
private static <T> boolean hasValidSize(T requestInst, Field field) {
    field.setAccessible(true);
    TelegramField telegramField = field.getAnnotation(TelegramField.class);

    try {
        if (telegramField.size() != field.get(requestInst).toString().length()){
            LOGGER.error("{} 필드길이 에러", field.getName());
            return false;
        }
    } catch (IllegalAccessException e){
        LOGGER.error("필드에 대한 잘못된 접근입니다.");
        return false;
    }

    return true;
}
~~~

### 5. JAVA - ENUM
http://blog.naver.com/PostView.nhn?blogId=kbh3983&logNo=220907314096&parentCategoryNo=&categoryNo=90&viewDate=&isShowPopularPosts=false&from=postView
> 상수들을 모아놓은 **클래스** 이다. 메서드, 생성자를 모두 가질 수 있다.  
런타임이 아닌 컴파일 타임에 모든 값을 알고 있어야 하기 때문에, 생성자가 private 로 제한된다. 싱글톤의 성질을 갖고 있다고 말할 수 있다.

1. Enum 인스턴스의 생성은 기본적으로 thread safe하다.

2. Enum 싱글톤은 Serialization을 스스로 해결한다  
    > 일반적인 싱글톤에서는 직렬화할 때, 싱글톤이 싱글톤이 아니게되는 문제가 발생한다. 
왜냐하면 직렬화 하려면 readObject()를 구현해야하는데, readObject()는 매번 새로운 인스턴스를 리턴하기 때문이다. 이러한 문제를 해결하기 위해서 몇가지 단계가 필요하다.

    1) 싱글톤 오브젝트를 직렬화 하기 위해 implements Serializable을 추가한다. 
    2) 모든 필드를 **transient** 로 선언한다   
    (특정 멤버변수가 serialization이 되는 것을 막는 것)
    3) readResolve() 메소드를 구현한다. 


3. reflection 을 통한 공격에도 안전
    - setAccessible 하면 private field 에도 접근 가능한데, ENUM은 이에 대한 대비를 이미 하고 있음 

### 6. Abstract Class VS Interface
> 둘다 선언만 있고 구현은 없는 클래스이다. 즉, 인스턴스를 생성해 낼 수 없고 추상클래스를 상속받는 자식클래스나, 인터페이스를 구현하는 클래스만이 인스턴스를 생성해 낼 수 있다.  
인터페이스는 어떠한 서비스의 필수 구현 사항 명세인 느낌이 강하다면,  
추상클래스는 자식이 부모 클래스 메서드를 가져다 쓰거나, 오버라이드 해서 사용하려는 느낌이 더 강하다.

**추상 클래스는 상속 개념이 들어간다는 것이 가장 큰 차이 -> 오버라이딩 (ISA)**  
부모에서 끌어쓰는 내용이 없고, 기능 구현을 명세할 것이면 인터페이스가 더 적합하다.

### 7. 자바 Throwable - Exception
![Exception-Class](https://user-images.githubusercontent.com/26560119/63499839-a8424580-c503-11e9-8be9-45bbef0b1d6a.png)


Throwable 을 상속받는 클래스는 Error, Exception 이다.  
에러는 시스템 레벨에서 발생하는 것이기 때문에 보통 시스템 자체에 변화를 주어 해결해야 한다.  
그러나 Exception은 개발자 선에서 처리할 수 있는 부분이다.

**Exception**은 많은 자식을 가지고 있는데 이는 두가지 그룹으로 나눠질 수 있다.  
**RuntimeException**은 Checked 예외와 Unchecked 예외를 구분하는 기준이다.

**Checked Exception** 은 컴파일 단계에서 확인하므로 반드시 컴파일 전에 예외처리가 필요하다. 
RuntimeException 빼고는 모든 예외가 이에 해당한다. 또한 Checked Exception 은 트랜잭션을 롤백하지 않는다. 미리 개발자가 인지하고 처리했을 것이라고 생각하기 때문인 것 같다.  
대표적인 예로는 IOException이 있다.

**Unchecked Exception** 은 무조건 RuntimeException 의 자식 클래스이다.  
명시적 처리를 강제하지 않고 예외시 트랜잭션을 롤백한다. NullPointException /IllegalArgumentException / IndexOutOfBoundException
/ SystemException 등이 런타임 예외에 해당된다.

런타임 예외처리도 중요하지만, Checked Exception 발생 시에는 무조건 Catch 할때 아무 처리도 하지 않는것 보다는 특별한 처리를 해 줄 수 있도록 노력해야 한다.


#### 참고 - Socket 사용 시 발생할 수 있는 에러

1. **SocketException**  (Checked Exception)  

    - 원인: read / write 시 상대방 socket close 된 경우

2. **IOException** - Broken pipe 등  (Checked Exception)  
수신자가 송신받은 데이터를 제때 처리하지 못하는 상황 (네트워크가 느리거나 서버의 CPU가 max인 경우 등) 에서 송신자가 계속 보내는 경우

3. **SocketTimeoutException** (IOException 자손 클래스)  