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

#### 클래스 로더
> JVM은 명령어를 클래스 파일에 저장한다. JVM 의 클래스 로더는 main 메서드가 호출될 메인 클래스부터 시작해서 필요할 때 클래스 파일을 로드한다. java.util.Scanner 같은 클래스를 이용하므로 이런 클래스도 로드하고, 이 클래스들이 각각 의존하는 클래스도 로드한다.

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

## 4. Wrapper Class 
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

## 5. ClassPath (클래스패스)
> 컴파일을 하면 java 파일에 선언된 클래스가 .class 파일 (bytecode)로 변환된다.  
javac 컴파일러는 항상 현재 디렉터리에서 파일을 찾지만, 자바 프로그램은 현재 디렉터리가 클래스패스에 있을때만 현재 디렉터리를 찾아본다, 그러나 따로 설정할 필요 없이 기본 설정으로 현재 디렉터리가 포함된다. (뻘짓만 안하면 가상머신이 클래스 로딩을 위해 클래스 파일 위치 찾는것에 무리가 없다는 내용)

**결국 클래스패스는 JVM이 클래스 로딩을 위해 .class 파일을 찾기 위한 경로이다**
- 클래스 파일을 파일 시스템에 저장하는 대신 JAR 파일이라는 아카이브 파일에 넣어서 사용할 수 있다. JDK의 일부인 jar 유틸리티로 이러한 아카이브를 만들 수 있다.

- jar 파일을 사용하려면, 클래스 패스를 지정해서 해당 파일의 위치를 컴파일러와 JVM에 알려야 한다.

## 6. 인터페이스
사용자 클래스 - 서비스 제공 클래스 간의 **계약**을 표현하는것이 인터페이스이다.  
인터페이스는 **구현(implements)** 하는 것이므로, 어떠한 필수 구현사항이 있다면 그것을 강제하는 느낌으로도 볼 수 있겠다. (구현 강제사항을 구현했는지는 컴파일러에서 검사한다)  
말그대로 계약이자 협박인 것이다. 실제 좋은 아키텍처에서는 의존성을 느슨하게 만들기 위해, 서비스 사용자는 서비스의 구현체가 아닌 인터페이스를 의존하게 된다. 

**"너 이 메서드 구현 안하면, 컴파일 안해준다 .."**

그리고 Java 8 이 나오기 전까지는 인터페이스에는 실행 블록이 없는 추상 메서드만 선언이 가능했다. 그러나 지금은 디폴트 메서드나 정적 메서드 선언이 가능하다. 

### 6.1 Java 8 에서 인터페이스에서 가질 수 있는 것들 
1.1 상수 필드
~~~java
public interface Parent{
    public static final MAX_VALUE = 100;
    public static final MIN_VALUE = 0;
}
~~~

1.2 추상 메서드
~~~java
public interface Parent{
    // 상수 필드
    public static final MAX_VALUE = 100;
    public static final MIN_VALUE = 0;
    
    // 추상 메소드
    public abstract void run();
}
~~~

1.3 디폴트 메서드
~~~java
public interface Parent{
    // 상수 필드
    public static final MAX_VALUE = 100;
    public static final MIN_VALUE = 0;
    
    // 추상 메소드
    public abstract void run();
    
    // 디폴트 메소드 : 실행 내용까지 작성이 가능하다.
    public default void setState(boolean state){
        
        if(state){
            System.out.println("현재 상태는 정상입니다");
        }else{
            System.out.println("현재 상태는 비정상입니다");
        }
        
    }
}
~~~

1.4 정적 메서드
~~~java
public interface Parent{
    // 상수 필드
    public static final MAX_VALUE = 100;
    public static final MIN_VALUE = 0;
    
    // 추상 메소드
    public abstract void run();
    
    // 디폴트 메소드 : 실행 내용까지 작성이 가능하다.
    public default void setState(boolean state){
        
        if(state){
            System.out.println("현재 상태는 정상입니다");
        }else{
            System.out.println("현재 상태는 비정상입니다");
        }
        
    }
    
    // 정적 메소드
    public static void change(){
        System.out.println("상태를 변경합니다.");
    }
}
~~~

### 6.2 타입 변환

2.1. 자동 타입 변환(Promotion)  

~~~java
Interface interface = new 구현체(); 
// 위와같은 경우 인터페이스에 선언된 메서드만 사용이 가능하다
~~~

2.2. 강제 타입 변환(Casting)  

구현 객체가 인터페이스 타입으로 **자동 타입 변환** 되는 경우  
인터페이스에 선언된 메소드만 사용이 가능한 반면,  

인터페이스 구현 클래스에 선언된 필드와 메소드를 사용해야 하는 경우에는,  
**강제 타입 변환** 을 이용하여 다시 구현 클래스 타입으로 변환해야한다.

~~~java
Interface interface = new 구현체(); 
구현체 imp = (구현체) interface;
~~~

강제 타입 변환시 instanceof 연산자로 확인하고 안전하게 강제 타입 변환을 할 것.

~~~java
if (car instanceof Bus){
    Bus bus = (Bus)car;
}
~~~

### 6.3 Runnable Interface
> 스레드의 테스크를 정의하려면 Runnable 인터페이스를 구현해야 한다.  
Runnable Interface는 메서드(run)를 한개만 갖는다.

~~~java
class ThreadTask implements Runnable {
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("Some Task");
        }
    }
}
~~~

이 태스크를 새로운 스레드에서 실행하려면 Runnable 인터페이스 구현체를 스레드 변수에 담아 실행시키면 된다.

~~~java
Runnable task = new ThreadTask();
Thread thread = new Thread(task);
thread.start();
~~~

## 7. 자바의 자료구조 
### 1. Array
~~~java
String[] names = new String[100];
~~~ 
**new 연산자로 배열 생성 시**
- 숫자 타입의 배열은 0으로 채워진다 
- boolean 의 배열은 모두 false 로 채워진다.
- 객체 배열은 null 참조 

### 2. JAVA collection framework
https://hackersstudy.tistory.com/26
https://onsil-thegreenhouse.github.io/programming/java/2018/02/18/java_tutorial_1-22/

![283](https://user-images.githubusercontent.com/26560119/64241191-2cea8600-cf3e-11e9-9b85-a3e9327e8cff.jpg)

> 컬렉션 프레임워크에는 자료 구조의 구현체가 있다.  
선택한 자료 구조에 독립적인 코드를 쉽게 작성할 수 있도록 컬렉션 프레임워크는 공통 인터페이스를 제공한다.  
대표적으로는 사용 용도에 따라 List, Set, Map, Queue 으로 나눌 수 있다.

### 1. 자료구조 인터페이스의 특징
> List, Set, Map, Queue 은 인터페이스이다.

1. List  
    구현 클래스 : ArrayList, LinkedList, Stack, Vector  
    간단 설명 : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.  

    검색이 빠른 구현체는 **ArrayList**  
    노드간 삽입이 빠른 구현체는 **LinkedList**

    **참고로** List 인터페이스는 리스트에서 n번쨰 요소에 접근하는 메서드를 제공한다.  
    Collection 클래스에서 이러한 접근을 지원한다는 사실을 나타내려면, RandomAccess 라는 인터페이스를 구현해야 한다.  
    **ArrayList**는 이러한 RandomAccess 인터페이스를 구현하지만,  
    **LinkedList** 는 RandomAccess 인터페이스를 구현하지 않는다.

    따라서 링크드리스트는 인덱스 접근이 불가하다는 사실을 기억!
2. Set   
    구현 클래스 : HashSet, TreeSet  
    간단 설명 : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다. 

    Set은 **순서** 가 없다는 것을 기억하자  
    그래도 Set은 Iterable 하기 떄문에 순회하여 값을 찾을 수 있다.  

    Set 에서 적합한 용도는 해당 요소가 해당 집합에 포함되는지를 조사할때 유용하다.

    평범한 싱황에서는 해시테이블을 이용하는 HashSet을 쓰면 되는데,  
    정렬된 순서대로 집합을 순회하려면, TreeSet 이 짱이다.

    TreeSet 은 이진검색트리 형태로 데이터를 저장하는데,  
    따라서 애초부터 정렬되어 트리에 저장된다,  
    이는 정렬과 검색에 뛰어난 성능을 보인다.  
    TreeSet은 이진트리를 개선한 레드블랙 트리로 구현되었다.  

    예를 들어 아래와 같은 소스가 있다면, 중복 없이 오름차순으로 정렬된 형태로 출력된다.
    ~~~java
    Set<Integer> set = new TreeSet<Integer>();

    ... ADD Integers to set

    Iterator<Integer> it = set.iterator(); // 반복자 생성

    while (it.hasNext()) {
        System.out.println(it.next());
    }

    ~~~

3. Map  
    구현 클래스 : HashMap, TreeMap, HashTable  
    간단 설명 : 키(key)와 값(value)의 쌍으로 이루어진 데이터의 집합이다.  
    순서는 유지되지 않고, 키는 중복을 허용하지 않으며 값의 중복을 허용한다.

    키를 정렬 순서로 방문해야 하는 이유가 없다면, HashMap 을 쓰는것이 낫다.  
    정렬 순서로 방문하려면 TreeMap 을 사용하면 된다.

4. Queue  
    Queue 는 요소를 뒤에서만 삽입하고 앞에서만 제거할 수 있다.  
    Deque 는 Double Ended Queue 의 약자로, 양끝에서 삽입 및 제거가 가능하다.


컬렉션 인터페이스는 모두 제너릭이며, 요소 타입에 대응하는 타입 파라미터를 받는다.  
그리고 코드를 작성할 때 가능하면 인터페이스를 사용하는 것이 좋다.
~~~java
List<String> list = new ArrayList<>();
~~~

### 2. Iterator
> 각 컬렉션에는 어떤 순서로 요소를 순회하는 메서드가 무조건 있다.  
Collection 의 부모 인터페이스인 Iterable<T>는 다음과 같은 메서드를 제공한다
~~~java
Iterator<T> iterator()
~~~
이 메서드는 모든 요소를 방문하는데 사용할 수 있는 반복자를 돌려준다.  
아래처럼 활용 가능하다.

~~~java
Collection<String> coll = new 어쩌고;
Iterator<String> iter = coll.iterator(); // 반복자 반환
while (iter.hasNext()) {
    String element = iter.next();
    // logic
}
~~~

아니면 더 간단하게 아래처럼 향상된 for 문도 가능하다.

~~~java
for (String element : coll) {
    //logic 
}
~~~

### 3. Stack, Queue, Deque, Priority Queue
- 스택은 한쪽 끝(top) 에서 요소를 추가하고 제거까지 하는 자료구조다 (LIFO)
- 큐는 한쪽 끝(tail) 에서 요소를 추가하고, Head 에서 요소를 제거한다.
- 스택 인터페이스는 없고 레거시 Stack 클래스만 있다 
- 위의 인터페이스 Stack, Queue, Deque 이 필요하지만 스레드 안전성을 신경쓰지 않아도 된다면, ArrayDeque 를 이용해 모두 구현할 수 있다.

**Stack 을 구현할 때** push, pop  
**Queue 를 구현할 떄** add, remove

병렬 프로그래밍 상황에서는 스레드 안전 큐를 사용하고,  
우선순위 큐는 요소를 무작위로 삽입해도 정렬된 형태로 꺼내게 된다.  
즉 remove() 시 현재 가장 작은 요소를 얻는다는 뜻이다. (작업 스케줄링에 사용)

## 8. Java 의 장단점
### 장점
- 운영체제에 독립적이다 (JVM 있음)
- 객체지향 언어이다. (추상화 ,캡슐화, 상속, 다형성)
- 자동으로 메모리 관리 해 준다 (JVM GC)
- 멀티스레딩 API 가 있어 쉽게 구현할 수 있다.
- 동적 로딩을 지원한다. 고치면 해당 클래스만 재컴파일

### 단점 
- 조금 느리다. (c++, c에 비해 느리단 말임, 당연히 portability 와 Trade-off)

## 9. 자바 언어 기본
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

#### 4.1 객체 조사하기
> 리플렉션을 이용하면 해당 객체의 필드와 타입을 나타내는 Field 객체를 얻을 수 있고, 해당 필드 값도 추출할 수 있다. 마찬가지로 Method 객체도 있어서 객체로 객체의 메서드를 호출할 수 있다. (대신 객체에 접근 가능하도록 setAccessible 해야 한다)

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

enum 선언하는 예를 보자
~~~java
public enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE}
~~~

위의 코드에서 enum 목록들은 모두 인스턴스이다.  
인스턴스처럼 꺼내 쓰는것이다. 열거의 각 인스턴스는 한번만 생성됨을 보장받는다.

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