[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Annotations
1. @PathVariable  
> 파라미터에 실려오는 변수를 나타내는 어노테이션 (Post 방식은 해당 x)
- 예시
~~~java
@GetMapping("/{id}")
public String show(@PathVariable Long id) {

}
~~~

2. @Configuration
> IOC 컨테이너에게 해당 클래스가 Bean 설정(구성) 클래스라는 것을 알려준다.

3. @Bean
> 개발자가 직접 제어 불가능한 외부 **라이브러리** 등을 Bean 으로 만들때 쓴다.
~~~java
@Bean
public Someclass someClass() {
    SomeClass a = new SomeClass();
    a.method;
    return a;
}
~~~
~~~java
@Bean(name="myClass")
~~~
위처럼 해당 라이브러리 객체를 반환하는 method를 만들고,  
그 위에 @Bean 어노테이션을 붙여 놓으면 된다.  
@Bean에 name을 따로 설정하지 않았으면 메서드가 Bean id로 등록된다.  
(위에서는 Bean ID가 'someClass')

4. @Component
> 개발자가 직접 작성한 Class를 Bean으로 등록하기 위해 사용된다.  
@Component도 이름을 지정할 수 있다.
~~~java
@Component(value="myClass")
~~~

5. @Autowired  
> type을 기준으로 의존성을 주입할 수 있도록 해준다.  
IOC 컨테이너에 등록된, Service, Component 등의 어노테이션이 작성된 객체를 주입해 준다.  

만약 다형성을 띄는 객체라면, 
~~~java
@Qualifier("Bean_이름")
~~~
위와 같은 어노테이션으로 해당 자리에 주입될 Bean을 명시해 주어야 한다.

6. @UtilityClass
> 유틸리티 클래스를 나타낸다. 특징은 생성자를 private 으로 바꿔서 static으로 쓰게끔 만들어 준다!
~~~java
@UtilityClass
public class TelegramUtil {}
~~~

7. 커스텀 어노테이션 만들기 
> 인터페이스를 생성해 준다. 실제 구현한 코드는 아래와 같음 
~~~java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface TelegramField {
    int size();
    int position();
}
~~~

> 그리고 클래스 실제 필드 선언에서 아래와 같이 사용하면 된다
~~~java
    @TelegramField(size=6, position=256)
    private String 거래_구분_코드;

    @TelegramField(size=20, position=262)
    private String 가맹점_번호;
~~~