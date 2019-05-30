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

## 3. @Component 와 @Autowired

## 4. Spring MVC