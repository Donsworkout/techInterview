[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. Why SpringBoot?

### 왜 스프링부트가 필요할까?  
- Embedded Tomcat (WAS, Servlet Container) 을 제공한다. 
- Maven pom.xml에서 의존 라이브러리의 버전을 일일이 지정하지 않아도 된다.  
(스프링 부트가 권장 버전을 관리한다)
- @SpringBootApplication 어노테이션으로 web.xml 파일을 대체한다.
    - @SpringBootApplication 는 @ComponentScan 과 @EnableAutoConfiguration 을 포함하고 있다. 
    - @ComponentScan 은 프로젝트 패키지에 있는 Bean을 찾아서 등록한다. 
    - @EnableAutoConfiguration 사용 가능성이 높은 Bean을 추측해 자동으로 등록한다.
- ContextLoaderListener / DispatcherServlet / CharacterEncodingFilter 설정은 Spring Boot에서는 추가하지 않아도 된다.

> ​스프링 부트는 classpath 상에 사용가능한 프레임워크와 이미 있는 환경설정을 바라본다.  
 이것들을 기반으로 스프링 부트는 애플리케이션을 이 프레임워크들과 함께 구성하는데 필요한 기본 환경설정을 제공한다. 이것을 **(A) Auto Configuration** 이라고 부른다.

#### (A) Auto Configuration
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

## 2. Servlet 과 JSP
### 1. 서블릿 (Servlet)
> 자바를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램 혹은 그 Spec  
자바 서블릿은 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종이다.  
자바로 구현된 [CGI](https://ko.wikipedia.org/wiki/%EA%B3%B5%EC%9A%A9_%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4) 라고 말할 수 있다.

#### 1.1 서블릿의 특징

- HTML 을 사용하여 요청에 응답한다.
- **Java Thread** 를 이용하여 동작한다.
- MVC 패턴에서 Controller로 이용된다.
- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다. 
- HTML 변경 시 Servlet을 재컴파일해야 하는 단점이 있다.

#### 1.2 서블릿 동작 방식
<img width="410" alt="993A7F335A04179D20" src="https://user-images.githubusercontent.com/26560119/63500386-ba70b380-c504-11e9-8638-7c39c0534f5d.png">

1. 클라이언트가 HTTP 요청시 이를 Servlet Container (WAS) 로 전송

2. 이를 전송받은 Servlet Container 는 ***HttpServletRequest, HttpServletResponse*** 의 객체들을 생성한다.

3. **web.xml** 은 사용자가 요청한 URL을 분석하여 어느 서블릿에 대해 요청을 한 것인지 찾습니다. (한개의 서블릿 당 한개의 컨트롤러 사상)

4. 해당 서블릿 스레드 (객체 아님) 에서 service 메소드를 호출한 후 요청의 POST, GET 여부에 따라 doGet() 또는 doPost()를 호출

5. doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse 객체로 응답.

6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 소멸


### 2. 서블릿 컨테이너 (aka WAS)
> 서블릿 컨테이너는 서블릿을 관리하며 네크워크 통신, 서블릿의 생명주기 관리, 스레드 기반의 병렬처리를 대행한다. 가장 대표적인 것으로는 아파치 톰캣(Tomcat)이 있다.

### 3. WS vs WAS (Apache VS Tomcat)

**웹서버** 는 클라이언트의 요청을 기다리고 요청에 대한 데이터를 만들어서 응답하는 역할을 한다. 이때 데이터는 정적인 데이터(html, css, 이미지등)로 한정된다. 말그대로 미리 정해진 자원들을 응답해 주는 것이다. 

**WAS** 클라이언트의 요청이 있을 때 ***내부 프로그램을 통해 결과를 만들어내고*** 이것을 다시 클라이언트에게 돌려주는 역할을 한다.
이는 WS의 기능 일부와 웹 컨테이너의 결합으로 다양한 기능을 컨테이너에 구현하여 다양한 역할을 수행한다. 

예전에는 static file 은 웹서버를 통해 제공하고, 동적인 기능들만 WAS 를 쓰는것이 효율적이라는 말이 있었는데, 톰캣은 5.5부터 **Httpd** 의 native모듈을 사용해서 스태틱파일을 처리하는 기능을 제공한다.  

이 경우 아파치와 톰캣이 같은 모듈을 사용하는 셈이니 성능에서 차이가 날 이유가 없다. 실제 테스트 한 결과를 봐도 톰캣에서 아파치 Native 모듈을 사용하는 것이 순수하게 아파치 Httpd만 사용하는 것과 비교해서 성능이 전혀 떨어지지 않는다. 

### 4. JSP (Java Server Page)
> 이는 웹 서버 어플리케이션의 발전속에 응답의 표현을 위해 자연스럽게 등장했다.

기존 서블릿 같은 경우 HttpServletResponse 객체에게 응답 html을 주려면 아래와 같이 상당히 귀찮은 작업이 필요했다.
~~~ java 
public class HelloServlet extends HttpServlet {

	public void doGet(HttpServletRequest req,
                        HttpServletResponse res)
			throws ServletException,IOException {

    

		res.setContentType("text/html;
                                    charset=UTF-8");

		PrintWriter out = res.getWriter();

		out.println("<HTML>");
		out.println("<BODY>");
		out.println("Hello World!!");
		out.println("</BODY>");
		out.println("</HTML>");
		out.close();

	}

}
~~~

그래서 등장한 도구가 JSP (루비의 ERB 같은 느낌이라고 보면 된다)
~~~jsp
<% @page import="java.util.Calendar" %> 
<% @page contentType="text/html; charset=UTF-8"%> 
<% String str = String.format("%tF",Calendar.getInstance()); %> <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"> 
<html> 
    <head>
        <meta http-equiv="Content-Type" 
        content="text/html; charset=UTF-8"> 
        <title>Insert title here</title> 
    </head> 
    <body> 
        오늘은 <%= str %><br/> 한시간만 참으면 점심.... 
    </body> 
</html>
~~~

아무튼 이렇게 기존 방식보다 html 작성이 훨씬 빠르게 되었다.  
내부적으로는 JSP 파일은 Tomcat이 Servlet으로 바꾸어서 돌린다고 한다.

## 3. 컨테이너 그리고 IOC / DI
#### 3.1 스프링 컨테이너 란?
컨테이너는 보통 인스턴스의 생명주기를 관리하며,  
생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이다.  
컨테이너는 개발자가 작성한 코드의 처리과정을 위임받은 존재이다.  
예를들어 서블릿 컨테이너는 서블릿을 생성하고 스레드를 이용해 사용자의 요청을 처리한다. 

이와 마찬가지로, 스프링에도 의존성 주입을 이용하여 어플리케이션 컴포넌트들을 관리하는 컨테이너가 있다. 이것이 바로 Spring Container 이다.

#### 3.2 IOC / DI
그리고 컨테이너는 아래와 같은 특징을 가진다

1. **IOC (Inversion Of Control - 제어의 역전)**

    개발자는 보통 객체지향 프로그래밍을 하면 New 연산자, 인터페이스 호출, 팩토리 호출방식으로 객체를 생성하고 소멸시킨다.  
    IOC란 이렇게 원래 개발자의 의무인 인스턴스의 생성부터 소멸까지의 객체 생명주기 관리를 컨테이너가 해 주는것을 말한다.

2. **DI (Dependency Injection - 의존성 주입)**

    IoC를 실제로 구현하는 방법으로서 의존성있는 컴포넌트들 간의 관계를 개발자가 직접 코드로 명시하지 않고 컨테이너인 Spring이 런타임에 찾아서(getBean) 의존성을 주입해 주는 것이다.

    - 인터페이스에 알맞는 Bean 을 주입하는 방법을 많이 사용한다.
    - 생성자 주입이 권장된다 (final 선언으로 immutable / 과한 책임의 가시성)


#### 3.2 스프링 컨테이너의 종류
1. **BeanFactory**

    DI의 기본사항을 제공하는 가장 단순한 컨테이너이다.  
    BeanFactory 는 빈을 생성하고 분배하는 책임을 지는 클래스  
    빈 자체가 필요하게 되기 전까지는 인스턴스화를 하지 않는다 (lazy loading)

2. **ApplicationContext**

    빈팩토리와 유사한 기능을 제공하지만 좀 더 많은 기능을 제공한다.  

    - 국제화가 지원되는 텍스트 메시지를 관리해 준다.

    - 이미지같은 파일 자원을 로드 할 수 있는 포괄적인 방법을 제공해준다.

    - 리스너로 등록된 빈에게 이벤트 발생을 알려준다.

    - Lazy Loading이 아니라 컨텍스트 초기화 시점에 모든 싱글톤 빈을 미리 로드한다.

## 4. AOP 와 Proxy

https://minwan1.github.io/2017/10/29/2017-10-29-Spring-AOP-Proxy/
> 모듈을 쉽게 사용할 수 있도록 해주는 것이라고 보는것이 편함  
특정 로직이 수평으로 흐른다면, 수직으로 원하는 흐름을 꽂아주는 방식이다.  
핵심로직을 구현한 코드를 컴파일하거나, 컴파일된 클래스를 로딩하거나, 또는 로딩한 클래스의 객체를 생성할 때, Proxy 객체를 통해 호출할 때 AOP가 적용됨

### AOP 기본
#### 1. AOP 용어
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

#### 2. Weaving 방식

1. 컴파일시에 Weaving하기
2. 클래스 로딩 시에 Weaving하기
3. 런타임시에 Weaving하기

> 1,2 번 같은 경우는 AspectJ라이브러리를 추가하여 구현할때 사용됨.  
런타임시 위빙같은 경우는 Spring-AOP 에서 사용하는 방식으로, 소스코드나 클래스 자체를 변경하는 것이 아니라 **프록시 객체** 를 사용하여 모듈을 Weaving 하고 비즈니스 로직을 실행한다.

***Proxy 를 이용한 런타임 위빙***

![스프링AOP](https://user-images.githubusercontent.com/26560119/63500528-f441ba00-c504-11e9-94cb-1aa87120984c.jpg)

#### 3. Spring-AOP
> 스프링은 자체적으로 프록시 기반의 런타임 AOP를 지원하고 있다.  
따라서 11-2 에서 살짝 설명한 것과 같이,  
Spring-AOP는 ***메서드 호출*** Joinpoint 만을 지원함.  
만약에 ***필드값 변경*** 과 같은 Joinpoint 를 사용하고 싶다면, AspectJ와 같이 다양한 Joinpoint를 지원하는 AOP프레임워크를 사용해야 한다.

또한 스프링은 3가지 방식으로 AOP를 제공한다.

- XML 스키마 기반의 POJO클래스를 이용한 AOP구현
- AspectJ에서 정의한 @Aspect 애노테이션 기반의 AOP구현
- 스프링 API를 이용한 AOP구현

**어떠한 방식을 사용하더라도 Proxy를 통한 메서드 호출만 AOP가 적용된다는것을 알아두자**

#### 4. 프록시를 생성하는 방식 
> 어쨌든 실행해야 하는 비즈니스 로직이 있는 클래스에 대해 무조건 Bean 객체가 생성되는데,  스프링 컨테이너가 지정한 Bean 객체에 대한 프록시 객체를 생성하고, 원본 Bean 객체 대신에 Proxy 객체를 사용하게 된다.  이때 프록시 객체를 생성하는 방식은 인터페이스 여부에 따라 두가지 방식으로 나뉜다.

1. JDK Dynamic Proxy  
- 이는 인터페이스 기반으로 프록시 객체를 생성하기 때문에, 인터페이스에 정의되어있지 않은 메서드에는 AOP가 적용되지 않는 점을 주의해야 한다.

2. CGLIB
- 인터페이스를 따로 구현하고 있지 않는 클래스는 CGLIB 방식으로 프록시를 생성한다.  
 이는 대상 클래스를 상속 받아 프록시를 구현하게 된다. 따라서 클래스가 final 인 경우는 프록시를 생성할 수 없고 private method 같은 경우에는 AOP 의 혜택을 받을 수 없다.


### Spring @Transactional 이 적용되지 않는 사례들
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

    

## 5. Spring MVC 동작 원리 

## 6. JPA vs MyBatis

### 6-1) 영속성 (Persistence)
- 데이터를 생성한 프로그램의 생명이 다하더라도, 데이터는 남아 있는 것
- 따라서 우리는 데이터베이스(DISK) 를 통해 영속성을 부여한다.
- Persistence Layer 는 데이터에 영속성을 부여해 주는 계층이다.
- 여기서 JDBC 없이 영속성을 갖게 해 주는 도구가, Persistence Framework  


![spring-jdbc-layer](https://user-images.githubusercontent.com/26560119/58633311-03a8d100-8323-11e9-81bf-1e18a68a6b34.png)

### 6-2) SQL Mapper 과 ORM
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

## 7. JAR vs WAR
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

## 7. 외장 톰캣 vs 내장 톰캣
- 웹 서버 위에서 다른 WAS 과 함께 운영해야 한다면 외장 톰캣
- 아니면 기존 레거시가 있거나 ..
- 그럴필요 없다면 내장 톰캣을 써도 좋다.  
- D2 Naver 의 동영상 플랫폼 개발 팀도 embed 톰캣을 쓰더라  
https://d2.naver.com/helloworld/5626759  
https://zepinos.tistory.com/51
## 8. 스프링 Log  
- 스프링은 LogBack, Log4j 등 여러 로그 라이브러리를 지원한다.
- Logback 이 Log4j 보다 10배이상 빠르고 메모리 효율성도 좋단다.

### Log Level (강한 로그 순서대로)
1) FATAL : 가장 크리티컬한 에러
2) ERROR : 에러
3) WARN : 에러는 아니지만 주의할 것 
4) INFO : 일반 정보
5) DEBUG : 일반 정보를 좀 더 상세히 (옵션을 켜야 보임)
6) TRACE : 에러 시 경로 추적 (스택 트레이스 같은 것)  

## 9. Maven vs Gradle

- 둘다 빌드 툴이다.
- 라이브러리 Dependency 를 관리한다.
> Gradle 이 더 좋아보임 (출시 시기도 늦고 ANT, MAVEN 장점만을 모았다고 함)

### GRADLE
- XML 기반의 Maven 빌드 설정보다 Gradle의 빌드 설정의 가독성이 더 좋다.
- Gradle의 빌드 성능이 Maven의 빌드 성능보다 더 뛰어나다
- **Groovy 기반의 DSL(domain-specific language)로 Gradle의 빌드 스크립트를 직접 작성할 수 있다.**

## 10. Spring TEST

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

## 11. JpaRepository save vs saveAll
- save는 트랜잭션 하나에 한개 insert
- saveAll은 트랜잭션 하나에 여러개 insert  
-> 트랜잭션 절약 
- saveAll 은 Batch 개수 설정도 가능
spring.jpa.properties.hibernate.jdbc.batch_size=4

## 12. 스프링 배치
- 일괄처리라는 뜻을 가지고 있음 
- WAS에서 대용량 처리로 인한 과부하 방지 
- 단발성 대용량 처리에 용이 (특정 시간 대용량 데이터 처리)
- SpringQaurtz 등 스케줄러와 같이 쓰는 경우가 많음

### 스프링배치 어플리케이션의 조건
- 대용량 데이터 - 배치 어플리케이션은 대량의 데이터를 가져오거나, 전달하거나, 계산하는 등의 처리를 할 수 있어야 합니다.
- 자동화 - 배치 어플리케이션은 심각한 문제 해결을 제외하고는 사용자 개입 없이 실행되어야 합니다.
- 견고성 - 배치 어플리케이션은 잘못된 데이터를 충돌/중단 없이 처리할 수 있어야 합니다.
- 신뢰성 - 배치 어플리케이션은 무엇이 잘못되었는지를 추적할 수 있어야 합니다. (로깅, 알림)
- 성능 - 배치 어플리케이션은 지정한 시간 안에 처리를 완료하거나 동시에 실행되는 다른 어플리케이션을 방해하지 않도록 수행되어야합니다.  

https://jojoldu.tistory.com/324
