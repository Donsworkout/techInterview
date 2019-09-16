[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)
## SpringBoot in Project

### 1. 스레드 / 스레드 풀  
https://postitforhooney.tistory.com/entry/JavaThread-Java-Thread-Pool%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-Thread%EB%A5%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0Thread-Runnable  

1. 스레드
> 스레드(thread)는 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 한 프로그램은 하나의 스레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다.

2. implements Runnable **VS** extends Thread  
> Runnable 은 자바의 다중상속에 대한 문제를 해결해 준다.

3. 스레드 풀  
https://limkydev.tistory.com/55

    - 병렬 작업 처리가 많아지면 Thread의 개수가 증가되고 어플리케이션의 성능이 저하됨
   
    - Thread Pool은 스레드를 미리 만들어 놓음
   
    - Thread Pool은 작업 처리에 사용되는 Thread를 제한된 개수만큼 정해 놓고 작업 큐(Queue)에 들어오는 작업들을 하나씩 Thread가 맡아 처리함
    
    \- Thread Pool도 과유불급이다. 너무 많이 만들어 놓으면 메모리 낭비 발생

### 2. ThreadPoolTaskExecutor 를 이용한 멀티쓰레드 구현
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

### 3. CompletableFuture

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


### 4. Properties -> Yaml
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

### 5. 여러 Yaml 설정파일 추가하기 
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

### 6. SpringApplicationBuilder
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

### 7. Spring Retry
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