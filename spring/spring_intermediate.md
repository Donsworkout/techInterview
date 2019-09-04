[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

### 1. ClassPath
https://effectivesquid.tistory.com/entry/%EC%9E%90%EB%B0%94-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%8C%A8%EC%8A%A4classpath%EB%9E%80
> 말 그대로 클래스를 찾기위한 경로, JVM이 프로그램을 실행할 때, 클래스파일을 찾는 데 기준이 되는 파일 경로를 말하는 것이다.

### 2. 필드 주입 vs 생성자 주입
https://warpgate3.tistory.com/entry/Spring-Constructor-Dependency-Injection-%EC%83%9D%EC%84%B1%EC%9E%90-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85  

**생성자 주입이 좋은 이유**  
1. 단일 책임 원칙
- 생성자가 무거워지면, 그만큼 한 클래스가 많은 책임을 떠맡는 것을 의미한다.
- 롬복을 쓰면 이 장점이 상쇄되긴 한다.
- final 키워드를 멤버 속성에 쓰므로, 필수 의존성을 명확히 해준다.
- 또한 final로 immutable 해지므로, 주입된 컴포넌트 변경이 불가해 진다.

2. POJO
- 생성자가 한개일 경우에 @Autowired 주석이 없어도 됨

### 3. properties 파일에서 한글 값 쓰기
> 무조건 한글은 유니코드 문자로 변환해서 써야 한다.
- 변환기 : 
https://huskdoll.tistory.com/39

~~~
김도현 : \uAE40\uB3C4\uD604
~~~

### 4. 스프링 스코프  
https://engkimbs.tistory.com/681

### 5. 스레드 / 스레드 풀  
https://postitforhooney.tistory.com/entry/JavaThread-Java-Thread-Pool%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-Thread%EB%A5%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0Thread-Runnable  

1. 스레드
> 스레드(thread)는 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 한 프로그램은 하나의 스레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다.

2. implements Runnable **VS** extends Thread  
- Runnable 은 자바의 다중상속에 대한 문제를 해결해 준다.

3. 스레드 풀  
https://limkydev.tistory.com/55

- 병렬 작업 처리가 많아지면 Thread의 개수가 증가됨

- 따라서 어플리케이션의 성능이 저하됨

- 이처럼 스레드 증폭을 막으려면 Thread Pool을 사용해야 함

- Thread Pool은 스레드를 미리 만들어 놓음

- Thread Pool은 작업 처리에 사용되는 Thread를 제한된 개수만큼 정해 놓고 작업 큐(Queue)에 들어오는 작업들을 하나씩 Thread가 맡아 처리함

- Thread Pool도 과유불급이다. 너무 많이 만들어 놓으면 메모리 낭비 발생

### 6. ThreadPoolTaskExecutor 를 이용한 멀티쓰레드 구현
https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/core/task/TaskExecutor.html
> TaskExecutor 라는 이름의 인터페이스를 사용하여 Thread의 개수를 자동으로 조절하고 ThreadPoolTaskExecutor 를 추상화하는 execute() 메소드를 사용해 멀티쓰레드로 작업을 처리합니다.

#### void execute(Runnable task)  
*Execute the given task.*
~~~
The call might return immediately if the implementation uses an asynchronous execution strategy, or might block in the case of synchronous execution.
~~~

execute() 함수의 인자로는 Runnable 인터페이스를 구현한 구현체가 들어가게 됨  
execute 는 Executor 인터페이스의의 메소드이다.   

~~~
Interface Executor - Interface TaskExecutor
// 부모 인터페이스 - 자식 인터페이스 
~~~

ThreadPoolTaskExecutor 는 위의 인터페이스를 구현하는 구현체이다.  
그리고 Interface AsyncListenableTaskExecutor 도 구현하고 있어 비동기 방식으로 작동한다.

~~~java
taskExecutor.execute(ctx.getBean(실행할.class));
~~~
그리고 위의 코드는 실행할 Bean의 run method 를 자동으로 실행한다.


설정 파일 예시 (/config/ThreadConfig.java)
~~~java
@Configuration
public class ThreadConfig {
    @Primary
    @Bean
    public TaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(50);
        executor.setMaxPoolSize(100);
        executor.setThreadNamePrefix("default_task_executor_thread");
        executor.initialize();
        return executor;
    }
}
~~~

### 6. 동시성 
#### 동시성 방어 원칙 
1. **단일 책임 원칙**  
동시성 관련 코드는 다른 코드와 분리해야 한다.

2. **따름 정리 - 자료 범위를 제한하라**  
코드 내 임계 영역(critical section)을 synnchronized 키워드로 보호  
스레드가 최대한 같은 자원을 참조하지 않도록 함  

3. **스레드는 가능한 독립적으로 구현**  
다른 스레드와 커뮤니케이션은 가급적 피하자

4. **유관 라이브러리**
- 스레드 환경에 안전한 collection 을 사용한다.
    > java.util.concurrent 패키지가 제공하는 클래스 적극 이용 (ConcurrentHashMap)
- 서로 무관한 작업을 수행할 때는 executor 프레임워크 사용 
- 가능하다면 스레드가 차단 되지 않는 방법을 사용 

### 7. CompletableFuture

Java7 부터는 Future 인터페이스를 통해서, 비동기 프로세스를 수행할 수 있었다.  
Future 클래스는 비동기 계산이 끝났는지 확인할 수 있는 isDone, 타임아웃 기간을 결정하고 결과를 출력하는 get 메소드 등이 있다.

JAVA8 에서는 복잡한 비동기처리를 선언형으로 이용할 수 있는 CompleteableFuture 를 제공하며, Stream API 나 Optional 같이 람다표현식과 파이프라인을 사용하여 비동기 작업을 조합할 수 있습니다.

일부 상황에서는 굳이 CompletableFuture 를 쓸 필요가 없어보이지만,  
병렬스트림과 달리 이를 이용한 방법은 executor 를 커스터마이징 할 수 있다.

https://m.blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220714398973&proxyReferer=https%3A%2F%2Fwww.google.com%2F

#### 장점

1) 명시적 쓰레드 선언없이 쓰레드를 사용할수 있다.

2) 함수형 프로그래밍방식으로 비동기적으로 동시성/병렬 프로그래밍을 가능하게 함으로서 의도를 명확하게 드러내는 함축적인 프로그래밍을 가능하게 한다.

3) 각 테스크마다 순서적 연결을 할수도 있고, 테스크의 예외처리도 가능하다.

4) 태스크간 결합이 용이함 



~~~java
Stream.of(PartnerCode.values())
    .forEach( partner ->
        completableFutures.add(
            CompletableFuture.supplyAsync(() ->
                getUserInfoForView(userCi, partner))
                .exceptionally(e -> {
                    LOGGER.info(e.toString());
                    return new UserInfoForView().setPartnerInfo(partner.getPartnerCode());
                })));


completableFutures.stream()
    .forEach(future -> 
        userInfoList.add(getUserInfoForViewFromFuture(future))
    );
~~~


### 7. Properties -> Yaml
> yml 파일은 계층 구조를 표현할 수 있는 장점이 있고,  
config 클래스를 따로 정의했다면, prefix, 필드명을 yml 파일 명세와 잘 일치시키면 자동으로 설정값을 주입할 수 있다. 게다가 List 나 Map 같은 구조로 설정 값을 주입시킬 수 있다.

1. pom.xml 파일에 의존성 추가 (spring-boot-configuration-processor)
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <version>2.1.6.RELEASE</version>
</dependency>
~~~

2. yml 파일 명세
~~~yml
#Hana
socket:
  serverAddress: 127.0.0.1
  serverPort: 8082
  timeout:
    checkRequest: 2000
    useRequest: 2000
    cancelRequest: 2000
    delivery: 2000
    checkSave: 2000
~~~

3. 설정 클래스 명세
~~~java
@Setter
@Getter
@Component
// prefix yaml 파일 명세와 잘 맞출 것 
@ConfigurationProperties(prefix="socket")
public class SocketConfig {
    private String serverAddress;
    private int serverPort;
    // timeout의 하위 attr 들을 맵 형태로 매핑할 수 있다.
    private Map<String, Integer> timeout;
}

~~~

### 8. 여러 Yaml 설정파일 추가하기 
> SpringApplicationBuilder 를 활용해 간단하게 설정할 수 있다.
~~~java
@SpringBootApplication
public class MyApplication {

    private static final String PROPERTIES =
            "spring.config.location="
                    +"classpath:/application.yml"
                    +",classpath:/config.yml";

    public static void main(String[] args) {
        new SpringApplicationBuilder(MyApplication.class)
                .properties(PROPERTIES)
                .run(args);
    }

}
~~~

### 9. SpringApplicationBuilder
[SpringApplicationBuilder 공식문서 확인](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/builder/SpringApplicationBuilder.html)

***그냥 앱 실행이라면 아래 방법이 제일 편함***
~~~java
public static void main(String[] args) {
    SpringApplication.run(MyApplication.class, args);
}
~~~

***그러나 실행시 Fluent builder API를 사용하기 위해 SpringApplicationBuilder 을 사용한다***  
+ 체인 방식의 메서드 Call 을 지원한다.
+ 계층을 표현할 수 있다

~~~java
new SpringApplicationBuilder() 
.showBanner(false) 
.sources(Parent.class) 
.child(Application.class) 
.run(args);
~~~

***아무튼 앱을 실행하는데에는 아래와 같은 방법들이 있다.***
~~~java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        // 방법 1 - static
        SpringApplication.run(Application.class);

        // 방법 2 - make instatnce
       SpringApplication app = new SpringApplication(Application.class);
       app.run(args);

       // 방법 3 - Builder
        new SpringApplicationBuilder()
            .sources(Application.class)
            .run(args);
    }
}
~~~

### 10. Spring Retry
https://github.com/spring-projects/spring-retry  
https://www.baeldung.com/spring-retry

> Spring Retry 는 실패한 작업에 대해 재시도 할 수 있도록 도와준다.  
이는 로직 등 개발자의 실수에 의한 에러보다는 일시적인 에러에 적합하다 (네트워크 장애 등)  
장애가 발생할 수 있는 메서드 위에 **@Retryable** 어노테이션을 이용하여, 특정 예외에 맞는 재시도 횟수, 재시도 BackOff 시간 등을 설정할 수 있다.

1. pom.xml 의존성 추가
~~~java
<dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>spring-retry</artifactId>
    <version>1.1.5.RELEASE</version>
</dependency>
~~~

2. Configuration에 @EnableRetry 어노테이션 추가
~~~java
@Configuration
@EnableRetry
public class AppConfig { ... }
~~~

3. 일시적 장애가 우려되는 메서드에 @Retryable 어노테이션 추가
~~~java
@Service
public interface MyService {
    @Retryable(
      value = { SQLException.class }, 
      maxAttempts = 2,
      backoff = @Backoff(delay = 5000))
    void retryService(String sql) throws SQLException;
    ...
}
~~~

4. 지정된 재시도 횟수만큼 시도했는데 실패했을때 대한 failback 메서드 작성
> 첫번째 인자로 발생한 예외를 넘겨주고, 그 뒤에는 순서대로 원래 메서드의 인자를 적어주면 된다. failback 매서드의 리턴값은 원래 메서드의 리턴값 형식과 같다.
~~~java
@Service
public interface MyService {
    ...
    @Recover
    void recover(SQLException e, String sql);
}
~~~

5. 테스트 등 필요하면 RetryTemplate 이용한다.
~~~java
@Configuration
public class AppConfig {
    //...
    @Bean
    public RetryTemplate retryTemplate() {
        RetryTemplate retryTemplate = new RetryTemplate();
         
        FixedBackOffPolicy fixedBackOffPolicy = new FixedBackOffPolicy();
        fixedBackOffPolicy.setBackOffPeriod(2000l);
        retryTemplate.setBackOffPolicy(fixedBackOffPolicy);
 
        SimpleRetryPolicy retryPolicy = new SimpleRetryPolicy();
        retryPolicy.setMaxAttempts(2);
        retryTemplate.setRetryPolicy(retryPolicy);
         
        return retryTemplate;
    }
}

~~~

### 11. Spring-AOP, Proxy
https://minwan1.github.io/2017/10/29/2017-10-29-Spring-AOP-Proxy/
> 모듈을 쉽게 사용할 수 있도록 해주는 것이라고 보는것이 편함  
특정 로직이 수평으로 흐른다면, 수직으로 원하는 흐름을 꽂아주는 방식이다.  
핵심로직을 구현한 코드를 컴파일하거나, 컴파일된 클래스를 로딩하거나, 또는 로딩한 클래스의 객체를 생성할 때, Proxy 객체를 통해 호출할 때 AOP가 적용됨

#### 11-1) AOP 용어
1. **Advice**  
언제 공통 관심 기능을 비즈니스 로직에 적용할 지 정의     
*예를들어,* '메서드를 호출 하기전에 트랜잭션 시작' 기능을 적용한다는 것을 정의하고 있는 것

2. **Joinpoint**  
Advice 를 적용 가능한 지점을 의미.  
메소드 호출, 필드값 변경 등이 Joinpoint에 해당

3. **Pointcut**  
Joinpoint의 부분집합으로서 실제로 Advice가 적용되는 Joinpoint를 나타냄  
스프링에서는 정규 표현식이나 AspectJ의 문법을 이용하여 Poincut을 재정의 할 수 있습니다.

4. **Weaving**  
Advice를 비즈니스 로직에 적용하는 행위   

5. **Aspect**  
여러 객체에 공통으로 적용되는 기능  
트랜잭션, 보안 등이 Aspect의 좋은 예

#### 11-2) Weaving 방식

1. 컴파일시에 Weaving하기
2. 클래스 로딩 시에 Weaving하기
3. 런타임시에 Weaving하기

> 1,2 번 같은 경우는 AspectJ라이브러리를 추가하여 구현할때 사용됨.  
런타임시 위빙같은 경우는 Spring-AOP 에서 사용하는 방식으로, 소스코드나 클래스 자체를 변경하는 것이 아니라 **프록시 객체** 를 사용하여 모듈을 Weaving 하고 비즈니스 로직을 실행한다.

11-2-1) Proxy 를 이용한 런타임 위빙 

![스프링AOP](https://user-images.githubusercontent.com/26560119/63500528-f441ba00-c504-11e9-94cb-1aa87120984c.jpg)

#### 11-3) Spring-AOP
> 스프링은 자체적으로 프록시 기반의 런타임 AOP를 지원하고 있다.  
따라서 11-2 에서 살짝 설명한 것과 같이,  
Spring-AOP는 ***메서드 호출*** Joinpoint 만을 지원함.  
만약에 ***필드값 변경*** 과 같은 Joinpoint 를 사용하고 싶다면, AspectJ와 같이 다양한 Joinpoint를 지원하는 AOP프레임워크를 사용해야 한다.

또한 스프링은 3가지 방식으로 AOP를 제공한다.

- XML 스키마 기반의 POJO클래스를 이용한 AOP구현
- AspectJ에서 정의한 @Aspect 애노테이션 기반의 AOP구현
- 스프링 API를 이용한 AOP구현

**어떠한 방식을 사용하더라도 Proxy를 통한 메서드 호출만 AOP가 적용된다는것을 알아두자**

#### 11-4) 프록시를 생성하는 방식 
> 어쨌든 실행해야 하는 비즈니스 로직이 있는 클래스에 대해 무조건 Bean 객체가 생성되는데,  스프링 컨테이너가 지정한 Bean 객체에 대한 프록시 객체를 생성하고, 원본 Bean 객체 대신에 Proxy 객체를 사용하게 된다.  이때 프록시 객체를 생성하는 방식은 인터페이스 여부에 따라 두가지 방식으로 나뉜다.

1. JDK Dynamic Proxy  
- 이는 인터페이스 기반으로 프록시 객체를 생성하기 때문에, 인터페이스에 정의되어있지 않은 메서드에는 AOP가 적용되지 않는 점을 주의해야 한다.

2. CGLIB
- 인터페이스를 따로 구현하고 있지 않는 클래스는 CGLIB 방식으로 프록시를 생성한다.  
 이는 대상 클래스를 상속 받아 프록시를 구현하게 된다. 따라서 클래스가 final 인 경우는 프록시를 생성할 수 없고 private method 같은 경우에는 AOP 의 혜택을 받을 수 없다.


 ### 12. Spring @Transactional 이 적용되지 않는 사례들
 > 스프링의 @Transactional annotation 은 Proxy Mode 와 AspectJ 모드로 동작한다.  
 일반적으로 많이 사용되는 것은 Proxy Mode 인데, 아무래도 Bean 이 아니라 프록시 객체를 이용하는 방법이다 보니, 의도치 않게 @Transactional 어노테이션이 동작하지 않는 사례들이 있다.

 1. **private method에 @Transactional 달았을때**
  
    Service Bean 에서 메서드를 호출하는 것이 아니라, Proxy 객체가 메서드를 호출하게 되므로 당연히 @Transactional 어노테이션이 적용되지 않는다.  
    이 부분은 SonarLint 같은 정적 코드분석기도 알려주는 부분이라 발견하기 쉽다.

 2. **메서드 안에서 내부 메서드 call** 

    self call 에 관한 부분이다.  
    내부 클래스에서의 동일 메소드를 호출 할 때에는 Proxy 객체가 아닌 this 를 이용해 메소드를 호출하기 때문이다. 프록시로 특정 메서드를 불러냈어도 그 메서드 흐름 안의 internal method 는 Bean 이 호출한다.  
    따라서 프록시 객체에 의해 동작하는 Spring-AOP는 내부 method를 호출하는지 까지는 관심이 없다. 처음 프록시로 불러낸 메서드까지가 Proxy 관할 구역임

    *아래와 같은 코드는 무용지물이다.  
    프록시는 자신이 호출한 메서드 그 하나만 인식하는 것이다.*
    
    ~~~java 
    @Transactional 
    public void cancel(String orgFlwNo) {
        cancelTransaction(orgFlwNo);
    }

    public void cancelTransaction(String orgFlwNo) {
        TradeRepository.cancelTrade(orgFlwNo);
        userPointRepository.savePoint(CURRENT_USER_ID)
    }
    ~~~

    *당연히 아래와 같은 코드도 무용지물이다.  
    어차피 프록시가 호출하지 않을 메서드이기 때문 (태어날때부터 self call이 예견되어 있다.)*  
    ~~~java 
    public void cancel(String orgFlwNo) {
        cancelTransaction(orgFlwNo);
    }

    @Transactional 
    public void cancelTransaction(String orgFlwNo) {
        TradeRepository.cancelTrade(orgFlwNo);
        userPointRepository.savePoint(CURRENT_USER_ID)
    }
    ~~~


    헷깔릴 수도 있는데, 프록시는 메서드를 불러주는 대리인이다.  
    대리인은 말그대로 다른 클래스에서 호출하는 것과 마찬가지다.  

    위 코드에서 cancel 메서드는 외부에서 부르게 되는 시나리오의 메서드이고, 아래 cancelTransaction은 cancel 내부에서 호출하고 실행해야 의미가 있는 내부 메서드이다. 
    *(위의 예제는 임시로 12.1 의 이유로 public scope로 되어 있음)*

    따라서 proxy 입장에서 내부에서 호출해야만 하는 메서드에 아무리 @Transactional 을 달아봐야 외부인인 proxy 는 내부 메서드 cancelTransaction 의 흐름을 가로챌 기회가 없는 것이다.

3. **Try Catch 구문 안에서 사용할때**

    @Transactional 어노테이션은 비즈니스 로직 실행 뒤 Exception을 인식하고 처리한다.  
    따라서 로직 안에서 try - catch 를 해버리면 @Transactional이 예외를 인식하기도 전에 메서드 내부에서 예외를 catch 하게 되어 예외시 ROLLBACK이 불가능하다.

    