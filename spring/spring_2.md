[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Spring 동작원리
https://asfirstalways.tistory.com/334


### 4. DispatcherServlet
#### Front Controller 인 DispatcherServlet 은 왜 생겨나게 되었나?

#### 1. 기존 컨트롤러 방식
~~~xml
<servlet> 
    <servlet-name>helloServlet</servlet-name>
    <servlet-class>com.java.HelloServlet</servlet-class>
</servlet>

<servlet-mapping> 
    <servlet-name>helloServlet</servlet-name>
    <url-pattern>hello</url-pattern>
</servlet-mapping>
~~~
> 기존 서블릿 - 컨트롤러는 직접 web.xml 에 서블릿을 등록하고, url 패턴과 mapping 해 주어야 했다. 그레서 FrontController 라는 존재를 만들어서 요청을 알맞는 컨트롤러 객체에 사상해 줄 수 있도록 새로운 방식이 고안되었음

#### 2. FrontController (DispatcherServlet 방식)

![front_ctrl](https://user-images.githubusercontent.com/26560119/64934463-f12cb600-d885-11e9-97d8-587083bfe1f7.jpeg)

<img width="665" alt="스크린샷 2019-09-16 오후 1 41 49" src="https://user-images.githubusercontent.com/26560119/64934734-c3e10780-d887-11e9-9797-a413307bb6b7.png">

