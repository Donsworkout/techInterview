[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Spring 기본 
https://asfirstalways.tistory.com/334

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

![서블릿 ](https://user-images.githubusercontent.com/26560119/63500397-be043a80-c504-11e9-9f91-94a0a72d3af2.jpeg)

1. 클라이언트가 HTTP 요청시 이를 Servlet Container (WAS) 로 전송

2. 이를 전송받은 Servlet Container 는 ***HttpServletRequest, HttpServletResponse*** 의 객체들을 생성한다.

3. **web.xml** 은 사용자가 요청한 URL을 분석하여 어느 서블릿에 대해 요청을 한 것인지 찾습니다.

4. 해당 서블릿에서 service 메소드를 호출한 후 클리아언트의 POST, GET여부에 따라 doGet() 또는 doPost()를 호출

5. doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse 객체로 응답.

6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 소멸

#### 1.3 WAS 와 서블릿 
- 바로 위 그림에서, init 은 서블릿 클래스가 메모리에 로드될때 실행된다

- init 된 서블릿은 클라이언트의 요청이 있을때마다 Thread 를 생성해서 서비스를 수행한다.   
- 서블릿 객체는 요청이 있을때마다 객체 자체를 생성하는게 아니다.

- WAS를 통해 컴파일되고 메모리에 적재된다

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

### 3. 컨테이너와 제어의 역전 (IOC) 
#### 3.1 스프링 컨테이너 란?
컨테이너는 보통 인스턴스의 생명주기를 관리하며,  
생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이다.  
컨테이너는 개발자가 작성한 코드의 처리과정을 위임받은 존재이다.  
예를들어 서블릿 컨테이너는 서블릿을 생성하고 스레드를 이용해 사용자의 요청을 처리한다. 

이와 마찬가지로, 스프링에도 의존성 주입을 이용하여 어플리케이션 컴포넌트들을 관리하는 컨테이너가 있다. 이것이 바로 Spring Container 이다.

그리고 컨테이너를 이해하기 전에 필요한 개념들이 있다.

1. **IOC (Inversion Of Control)**

    개발자는 보통 객체지향 프로그래밍을 하면 New 연산자, 인터페이스 호출, 팩토리 호출방식으로 객체를 생성하고 소멸시킨다.  
    IOC란 이렇게 원래 개발자의 의무인 인스턴스의 생성부터 소멸까지의 객체 생명주기 관리를 컨테이너가 해 주는것을 말한다.

2. **DI (Dependency Injection)**

    IoC를 실제로 구현하는 방법으로서 의존성있는 컴포넌트들 간의 관계를 개발자가 직접 코드로 명시하지 않고 컨테이너인 Spring이 런타임에 찾아서(getBean) 의존성을 주입해 주는 것이다.

컨테이너는 위의 두가지 기능을 사용하여, Spring Bean 의 생명주기를 관리한다.

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

### 4. 스프링 웹 어플리케이션 동작원리 
https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/

![PlDF42i](https://user-images.githubusercontent.com/26560119/63500439-ceb4b080-c504-11e9-84a4-98dc31402e3b.png)


1. 웹 어플리케이션이 실행되면, Tomcat(WAS)에 의해 web.xml 이 로딩된다.

2. web.xml에 등록되어 있는 **ContextLoaderListener(Java Class)** 가 생성된다. ContextLoaderListener 클래스는 ServletContextListener 인터페이스를 구현하고 있으며, **ApplicationContext** (스프링 컨테이너) 를 생성하는 역할을 수행한다.

3. 생성된 ContextLoaderListener는 root-context.xml을 loading한다.

4. root-context.xml에 등록되어 있는 Spring Container가 구동된다. 이 때 개발자가 작성한 비즈니스 로직에 대한 부분과 DAO, VO 객체들이 생성된다.

5. 클라이언트의 요청 

6. DispatcherServlet(Servlet)이 생성된다. DispatcherServlet은 FrontController의 역할을 수행한다. 클라이언트로부터 요청 온 메시지를 분석하여 알맞은 PageController에게 전달하고 응답을 받아 요청에 따른 응답을 어떻게 할 지 결정만한다. 실질적은 작업은 PageController에서 이루어지기 때문이다. 이러한 클래스들을 HandlerMapping, ViewResolver 클래스라고 한다.

7. 두번째 Spring Container가 구동되며 응답에 맞는 PageController 들이 동작한다. 이 때 첫번째 Spring Container 가 구동되면서 생성된 DAO, VO, ServiceImpl 클래스들과 협업하여 알맞은 작업을 처리하게 된다.

#### 유심히 봐야할 특징
- 어플리케이션이 실행되고, 클라의 요청을 받아들이기 전에 첫번째 스프링 컨테이너가 구동되는데, 이때 **Service 나 DAO, VO 객체** 들이 먼저 생성된다.

- 그리고 클라이언트의 요청 이후 디스패처 서블릿이 컨트롤러 맵핑을 마치고 나서 두번쨰 스프링 컨테이너가 구동되고 이제서야 **PageController** 들이 동작한다. 