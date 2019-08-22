[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

### 1. final 과 static final 의 차이
1. static
- 자바에서 static은 클래스 멤버를 설정하는 키워드이다.
- JVM 클래스 로딩 시, 클래스의 모든 정보다 JVM class 영역에 올라가므로 static 키워드를 썼다면 이때 static 맴버가 적재된다.

2. final 
- 상수, 메서드, 클래스에 사용가능하다.
- 상수라면 값 고정, 메서드라면 오버라이딩 금지, 클래스라면 상속 금지 

3. static final 
- 객체가 아니라 클래스에 존재하는 단 하나의 상수이다.
~~~java
private static final Logger LOGGER = LoggerFactory.getLogger(sth.class);
~~~

### 2. DataInputStream / DataOutputStream
https://m.blog.naver.com/PostView.nhn?blogId=highkrs&logNo=220474124970&proxyReferer=https%3A%2F%2Fwww.google.com%2F

### 3. throw 와 throws
https://vitalholic.tistory.com/246

### 4. Reflection
> 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법

[BeanFactory](http://wonwoo.ml/index.php/post/1571) 라는 Spring Container 는 어플리케이션이 실행한 후 객체가 호출 될 당시 객체의 인스턴스를 생성하게 되는데 그 때 필요한 기술이 Reflection 이다.

아래 링크에 자세히 나와 있다.  
https://gyrfalcon.tistory.com/entry/Java-Reflection

실제 리플렉션을 이용하여 어노테이션으로 값을 설정하는 행위를 해보았는데,  
실제 프로젝트에서는 아래와 같이 활용하였다.
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
> 상수들을 모아놓은 **클래스**, 메서드 생성자를 모두 가질 수 있다.  
런타임이 아닌 컴파일 타임에 모든 값을 알고 있어야 하기 때문에, 생성자가 private 로 제한된다. 싱글톤의 성질을 갖고 있다고 말할 수 있다.

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

#### 추상 클래스는 상속 개념이 들어간다는 것이 가장 큰 차이 -> 오버라이딩 (ISA)

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