[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Libraries
1. swagger2
[확인하기](https://lng1982.tistory.com/314)
> API 문서화 / 테스팅 라이브러리  

2. logback
[확인하기](https://taetaetae.github.io/2017/02/19/logback/)
> 로깅 라이브러리, 성능 좋음, 스프링부트에서 기본 제공중  
lombok의 @Slf4j 어노테이션 사용가능 

~~~java
import com.example.demo.domain.User;
import com.example.demo.repository.UserRepository;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@AllArgsConstructor
@RestController
@Slf4j
@RequestMapping("/api/users")
public class ApiUserController {
    private final UserRepository userRepository;

    @GetMapping("")
    public String getAllUsers(){
        return "200 success";
    }

    @PostMapping
    public User create(String name, String info){
        User user = new User(name, info);
        try {
            return userRepository.save(user);
        }catch (RuntimeException ex) {
            log.warn(ex.getCause().getMessage());
            return user;
        }
    }
}
~~~
3. freemarker
> 템플릿 엔진  
아래는 application.properties 관련 설정
~~~
spring.freemarker.template-loader-path=classpath:/templates
spring.freemarker.suffix=.ftl
~~~
pom.xml
~~~xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-freemarker -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-freemarker</artifactId>
    <version>2.1.3.RELEASE</version>
</dependency>

~~~