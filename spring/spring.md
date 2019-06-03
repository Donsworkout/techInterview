[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. 스프링부트 vs 스프링 MVC vs 스프링

### 1) 왜 스프링부트가 필요할까?  
- 그냥 Spring MVC 를 사용하면, 컴포넌트 스캔 / 디스패쳐 서블릿 / 뷰 리졸버 등을 모두 설정해야 한다.

- 하이버네이트 / JPA를 사용해야 할 때도 xml을 통해 구성해야 할 것이 많다.

> 아래와 같은 생각에서 스프링 부트를 탄생시켰다.
- Hibernater jar 가 클래스 패스상에 있을 경우 Data Source를 자동으로  구성하는 것은 어떨까?

- Spring MVC jar 가 클래스패스상에 있을 경우  Dispatcher Servlet 을 자동으로 구성하는 것은 어떨까?

> ​스프링 부트는 클래스패스상에 사용가능한 프레임워크와 이미있는 환경설정을 바라본다. 이것들을 기반으로 스프링 부트는 애플리케이션을 이 프레임워크들과 함께 구성하는데 필요한 기본 환경설정을 제공한다. 이것을 **Auto Configuration** 이라고 부른다.

### 2) Spring Boot AutoConfiguration
- 스프링 MVC에서 사용하는 의존성은 아래와 같다. 
- Spring MVC, Json 위한 Jackson, Hibernate validator, Log4j
- 버전이 굳이 명시되어 있는것을 알 수 있다.
~~~xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>4.2.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.3</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.0.2.Final</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
~~~

- Spring Boot Starter App 을 이용하면 아래 모든 의존성이 자동 세팅된다.
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

- Spring: core, beans, context, aop
- Web MVC: (Spring MVC)
- Jackson: for JSON Binding
- Validation: Hibernate Validator, Validation API
- **Embedded Servlet Container: Tomcat**
- Logging: logback, slf4j


## 2. DI, IOC, AOP

### 1. IOC (Inversion of Control)

- 제어의 역전, 메소드나 객체의 호출작업을 외부에 맡기는 것이다.
- 개발자는 프레임워크가 필요한 부품을 개발하고 흐름은 프레임워크가 결정하는 것
- 서블릿 컨테이너 (톰캣, WAS) 가 요청 들어오면 알아서 지지고 볶고 하는 것이 바로 이것 
- 프레임워크가 코드를 통제한다면, 라이브러리는 코드에 의해 통제된다.

#### cf) Container
> 컨테이너(Container)는 보통 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이라 할 수 있다.  
다시말해, 컨테이너란 당신이 작성한 코드의 처리과정을 위임받은 독립적인 존재라고 생각하면 된다. 컨테이너는 적절한 설정만 되어 있다면 누구의 도움 없이도 프로그래머가 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.

#### Spring에서의 IoC
- Spring 의 IOC Container은 java의 일반 POJO의 생명주기를 관리한다. 

### 2. DI (Dependency Injection)
- 우리가 의존적인 객체를 직접 객체를 생성하고 제어하는 것이 아님
- IoC가 일어날 때, 특정 객체에 필요한 객체를 외부에서 결정하고 연결시킨다.
- 모듈간 의존 관계가 느슨하게 있다가, 사용 시 의존관계를 확정하는 것이다.
- 우리는 따라서 해당 클래스의 기능을 묶어둔 인터페이스를 가져다 쓰면 된다.
- JPA Repository 상속받아 명시된 Interface도 실제 내용조차 적지 않는다. 
- 말그대로 구현체(impl) 따로 있고, 우리는 그저 인터페이스를 가져다 쓰는 것 
- GoF의 Strategy Pattern 이 이를 뒷받침한다.  
참고 : 
https://itstory.tk/entry/Gof-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-Strategy%EC%A0%84%EB%9E%B5-%ED%8C%A8%ED%84%B4

### 2.1. DI의 유형
- 생성자로 주입하자! (field 선언은 private final 로)
- POJO 스러울수록 좋다.
- 생성자가 하나이면 심지어 @Autowired 안써도 된다.


~~~java
public class SpringTest {
// final 키워드는 생성자 초기화 필수라는 뜻 (세터 불가)
      private final SpringTestService service;
     
// 필드 주입
      @Autowired
      private Spring spring;
// 생성자 주입 
      @Autowired
      public SpringTest(SpringTestService service){
            this.service = service;
      }
}
~~~
~~~java
public class FooService {
 
    private FooFormatter fooFormatter;
// 아래가 setter (method) 를 통한 주입
    @Autowired
    public void setFooFormatter(FooFormatter fooFormatter) {
            this.fooFormatter = fooFormatter;
    }
}
~~~
> 내가 프로젝트에서 필드 주입을 썼는데 그것이 좋은 방법은 아니란다. 단일 책임 원칙에 위배되는 짓을 하고있을지 모르기 떄문 ..  
생성자는 파라미터가 많아질수록 더 드러나므로 경각심이 드러난다.

### 3. AOP (Aspect Oriented Programming)
> 자주 쓰는데 계속 코드에 나타나는 애들 관리하기 위한 것으로 그 예에는,
log나 권한체크, 예외처리 등이 있다.  
AOP 개발방식에는 몇가지가 있고, AOP를 설정하는 방법에는  
@AspectJ 어노테이션을 이용한 방법과 XML을 이용한 방법 2가지가 있습니다.

- 하나 이상의 포인트컷이 필요하다. 포인트컷은 그냥 Aspect를 받을 놈이다.
~~~java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
 
@Aspect
public class Test_AspectJ {
 
    @Pointcut("execution(* com.ktds.aop.AspectTestController.*Before())")
    public void human() {
    }
 
    @Pointcut("execution(* com.ktds.aop.AspectTestController.*After())")
    public void human1() {
    }
 
    @Pointcut("execution(* com.ktds.aop.AspectTestController.*Around())")
    public void human2() {
    }
 
    @Before("human()")
    public void before_test() {
        System.out.println("1. @Before어드바이스에서 출력");
    }
 
    @After("human1()")
    public void after_test() {
        System.out.println("4. @After어드바이스에서 출력");
 
    }
 
    @Around("human2()")
    public Object test_Method(ProceedingJoinPoint joinPoint) throws Throwable {
        String signature = joinPoint.getSignature().toString();
        System.out.println(signature + " 시작");
        long start_time = System.currentTimeMillis();
        try {
            Object result = joinPoint.proceed();
            return result;
        } finally {
            long end_time = System.currentTimeMillis();
            System.out.println(signature + " 종료");
            System.out.println(signature + " 실행시간" + (end_time - start_time)+ "ms");
        }
    }
}

~~~

## 3. Spring MVC
![다운로드](https://user-images.githubusercontent.com/26560119/58632046-7a43cf80-831f-11e9-96d8-f9f90ce7d985.png)

1. Client 요청은 항상 Dispatcher Servlet 이 가로챈다.
2. Dispatcher Servlet이 HandlerMapping 에게 요청을 전달
3. HandlerMapping은 요청에 맞는 알맞는 컨트롤러 객체를 리턴
4. DispatcherServlet이 컨트롤러 객체를 HandlerAdapter에 전달하여 알맞는 컨트롤러 메소드를 호출한다. 
5. 컨트롤러가 서비스 - DAO - Mybatis - DBMS 등을 거쳐 처리한 결과를 모델에 담아 DispatcherServlet에게 다시 리턴한다.
6. DispatcherServlet은 ViewResolver 객체를 이용하여 뷰를 검색한다.
7. ViewResolver은 모델에 담긴 View 이름을 이용해서 View 객체를 찾거나 새로 만들어서 DispatcherServlet에게 다시 전달하고,
8. DS 는 해당 View 객체에게 Client 에게 해당 결과를 보여주도록 한다. 

## 4. JPA vs MyBatis

### 4-1) 영속성 (Persistence)
- 데이터를 생성한 프로그램의 생명이 다하더라도, 데이터는 남아 있는 것
- 따라서 우리는 데이터베이스(DISK) 를 통해 영속성을 부여한다.
- 프로그램 내 계층 중에도 Persistence Layer 가 있다.
- Persistence Layer 는 데이터에 영속성을 부여해 주는 계층이다.
- 여기서 JDBC 프로그래밍 없이 영속성을 갖게 해 주는 도구가, Persistence Framework  


![spring-jdbc-layer](https://user-images.githubusercontent.com/26560119/58633311-03a8d100-8323-11e9-81bf-1e18a68a6b34.png)

### 4-2) SQL Mapper 과 ORM
Persistence Framework 는 두가지가 있다.
#### 1. SQL Mapper  
- SQL **<- mapping ->** Object 필드 
- 대표적인 예로는 Mybatis 이고, 실제 SQL을 명시해 주어야 한다.
- 직접 SQL을 많이 만져야 하는 쿼리가 있는 경우에 좋다.  
- XML 파일에 쿼리 쓰고 @Autowired 하는 방식 
> SQL Mapper의 Query는 xml로 관리되어지기 때문에 유지보수 과정에서 문제가 발생했습니다. 빌드 과정에서 오류를 발생시키지 않기 때문에 최악에는 SQL Query과 관련된 모든 Controller/Business/DAO Layer를 찾아서 유지보수 해야했습니다.


#### 2. ORM (Object - Relation Mapping)
- 객체와 RDB 테이블의 데이터를 자동으로 매핑해 주는 도구
- SQL Query 대신 직관적인 method 로 트랜잭션 할 수 있다. 
- SQL을 자동으로 생성한다. 
- Persistence API 라고 할 수 있다. 
- Hibernate 는 JPA 의 구현체이다. 

#### 2-1) JPA 그리고 Hibernate 
<img width="996" alt="overall_design" src="https://user-images.githubusercontent.com/26560119/58634478-5a63da00-8326-11e9-811e-02626d70d119.png">

- JPA 는 자바 어플리케이션에서 데이터베이스를 사용하는 방식을 정의한 인터페이스이다.
- Hibernate 는 JPA를 구현한 구현체이다.

#### 3. ORM의 장점과 단점 
#### 장점 :
- 객체지향적인 코드로 직관적이고, 비즈니스 로직에 더 집중할 수 있다.
- DBMS 에 종속적이지 않다. 
- 재사용 및 유지보수의 편리성이 증가한다. 
- JPA 경우 @Query 어노테이션을 사용하여 SQL을 직접 쓸 수 있으나, 복잡한 것 뺴고는 JPQL 이용하도록 함 

#### 단점 : 
- JPA의 내장 메소드로 해결되지 않는 쿼리가 많다.
- 자주 사용되는 대형 쿼리는 별도의 튜닝이 필요한 경우가 있다. 
- 최적화된 SQL 쿼리를 사용할때는 MyBatis 가 낫다. (배우기 어렵)
- 아래 링크 참조   
https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html
- ORM에 대한 개발자들의 생각   
https://okky.kr/article/286812

## 5. JAR vs WAR
> class < jar < war < ear  

### 5-1) JAR (Java Application Archive)  
- java class 파일 및 리소스파일(이미지, 텍스트 등)의 컨테이너이다.
- jar 파일이 독립실행 되려면, 클래스 중 하나가 기본 클래스로 정의된다. 
- 실행 가능한 jar 파일은 아래 명령어로 실행된다. (spring 지원)
~~~
java -jar foo.jar
~~~

### 5-2) WAR(Web Application aRchive)
- 웹 어플리케이션을 저장하고 압축해 놓은 파일
- html, JSP, 서블릿 소스 등이 컴파일 되어 저장되어 있다. 
- war는 WAS 위에서 돌아갈 수 있다.  
- Tomcat 위에 얹어서 디플로이한다. 

## 6. 외장 톰캣 vs 내장 톰캣
- 별도의 WAS를 구현하고 그 위에 다른 application 과 함께 운영해야 한다면 외장 톰캣
- 그럴필요 없다면 내장 톰캣을 써도 좋다.  
- D2 Naver 의 동영상 플랫폼 개발 팀도 embed 톰캣을 쓰더라  
https://d2.naver.com/helloworld/5626759

## 7. 스프링 Log  
- 스프링은 LogBack, Log4j 등 여러 로그 라이브러리를 지원한다.
- Logback 이 Log4j 보다 10배이상 빠르고 메모리 효율성도 좋단다.

### 7-1) Log Level (강한 로그 순서대로)
1) FATAL : 가장 크리티컬한 에러
2) ERROR : 에러
3) WARN : 에러는 아니지만 주의할 것 
4) INFO : 일반 정보
5) DEBUG : 일반 정보를 좀 더 상세히 (옵션을 켜야 보임)
6) TRACE : 에러 시 경로 추적 (스택 트레이스 같은 것)  

## 8. Maven vs Gradle

- 둘다 빌드 툴이다.
- 라이브러리 Dependency 를 관리한다.
> Gradle 이 더 좋아보임 (출시 시기도 늦고 ANT, MAVEN 장점만을 모았다고 함)

### GRADLE
- XML 기반의 Maven 빌드 설정보다 Gradle의 빌드 설정의 가독성이 더 좋다.
- Gradle의 빌드 성능이 Maven의 빌드 성능보다 더 뛰어나다
- **Groovy 기반의 DSL(domain-specific language)로 Gradle의 빌드 스크립트를 직접 작성할 수 있다.**

## 9. Spring TEST

- 레일즈는 RSpec, Capybara 등 라이브러리 존재
- 카피바라로는 패스트원 할떄 로그인, 메뉴진입 등 기본적인 View 기반 테스트 진행 

> 스프링부트는 처음 빌드 설정파일에 spring-boot-starter-test 넣으면 다 설치됨.  
아래와 같은 것들이 포함된다고 한다.

- JUnit — 자바 표준 단위 테스트 프레임워크
- Spring Test — 스프링 부트 애플리케이션 테스트 지원을 위한 유틸리티
- AssertJ — 어셜선 라이브러리
- Hamcrest — 유용한 매처를 지원하는 객체 라이브러리
- Mockito — 자바 모킹 프레임워크
- JSONassert — JSON 어셜션 라이브러리
- JsonPath — JSON 구조를 탐색할 때 유용한 라이브러리

## 10. JpaRepository save vs saveAll
- save는 트랜잭션 하나에 한개 insert
- saveAll은 트랜잭션 하나에 여러개 insert  
-> 트랜잭션 절약 
- saveAll 은 Batch 개수 설정도 가능
spring.jpa.properties.hibernate.jdbc.batch_size=4