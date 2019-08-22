[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Clean Architecture
https://medium.com/@younghyun/%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%8C%ED%8A%B81-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-vs-%EB%8F%84%EB%A9%94%EC%9D%B8-236c7008ac83

### 1. 프레임워크로부터 독립해라 
> 아키텍쳐는 프레임워크에 의존하면 안된다! 최소한의 노력으로 새로운 프레임워크를 적용할 수 있어야 한다.

#### Comment  
    프레임워크가 주는 생산성과 클린 아키텍쳐와의 균형을 잘 찾는 것이 중요한것 같다.  
    AOP, DI 같은 경우도 사실은 스프링 프레임워크의 기능인데, 이를 쓰지않고 개발하는 것은 곧 바퀴를 새로 만드는 꼴이 된다. 과유불급.

### 2. 세부 구현으로부터 독립해라 
> SW 시스템의 핵심 모듈은 UI, 데이터베이스, 프레임워크, 라이브러리 등의 변경에 영향을 받지 않아야 한다.


### 3. 내부 레이어는 상위/외부 레이어의 어떤 것도 알아서는 안된다
> 그 결과로 모든 의존성은 내부 레이어를 향해야 한다.  
(보통 DB에 가까운 LEVEL)

### 4. Entity 와 Use Case
> 아키텍처에 대해서 얘기할 때 이것들이 존재하는 레이어가 가장 중요하다.  
그리고 이 레이어는 세부 (구현) 사항을 담고 있는 레이어의 변경에 영향을 받지 않아야 한다.

### 5. Adapter 와 Converter 의 필요성
Adapter 와 Converter 는 각각의 레이어가 협력을 할 때 레이어 내부의 세부 사항이 다른 레이어로 전파되지 않도록 하는 역할을 한다.  

즉, 각각의 레이어가 필요로 하는 그들만의 데이터 모델을 외부 레이어로 전달하거나 외부 레이어에서 받아 올 때 외부 또는 내부 레이어의 데이터 모델의 격리를 위해서는 외부 레이어의 데이터 모델을 내부 레이어의 데이어 모델로 변환하는 그리고, 내부 레이어의 데이터 모델을 외부 레이어의 데이터 모델로 변환하는 기능을 담당하는 녀석들이 필요한데 이 두 녀석이 Adapter 와 Converter 이다.  

이를 통해 얻을 수 있는 것은 적절한 의존성이다. 이 둘이 레이어간 완충재 역할을 한다고 보면 되겠다. 그리고 이 녀석들을 통해 DDD에서 말하는 Bounded Context 가 좋은 형태로 구현된다고 볼 수도 있겠다.

#### Comment  
    데이터 전달 시, 맘대로 계층을 뛰어넘지 말고 한 계층씩 거쳐 가라는 말인것 같다.

### 6. 의존성 역전의 법칙 (DIP)
http://wonwoo.ml/index.php/post/1717
> 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.

***???? 무슨 말이지?***

예를들어,  
어떤 바이트 데이터를 읽어와 특정한 데이터로 결과를 반환하는 것으로 이야기를 해보자.

**고수준 모듈**  
- 바이트 데이터를 읽어 와서 특정한 데이터로 결과를 반환한다.

**저수준 모듈**  
- 파일에서 바이트 데이터를 읽어 온다.
- Json 데이터로 결과를 반환한다.


의존성 역전의 법칙을 어기는 코드의 예
~~~java 
public class Response {

  private JsonConverter jsonConverter = new JsonConverter();

// 고수준 모듈 (JsonConverter 에게 의존하고 있다)
  public String response()  {
    byte[] bytes = null; 
    return jsonConverter.convert(bytes);
  }

}

// 저수준 모듈 
class JsonConverter {

  public String convert(byte[] bytes) {
    //json ...
    return "json";
  }
}

~~~

이를 해결한 예 (인터페이스 사용)
~~~java
public class Response {

  private Converter converter = new JsonConverter();

// JsonConverter 가 아니라 Converter에 의존한다
  public String response() {
    byte[] bytes = null; 
    return converter.convert(bytes);
  }

}

interface Converter {

  String convert(byte[] bytes);

}

class JsonConverter implements Converter {

  @Override
  public String convert(byte[] bytes) {
    //json ...
    return "json";
  }
}
~~~

여기다가 DI까지 추가하면 금상첨화 (스프링)

### 7. 경계(boundary) 간의 데이터 전달 
레이어간 객체를 전달하는 것에는 항상 주의를 기울여야 한다. 전달되는 객체는 항상 고립되어야 함. 즉, 언어의 기본 형태를 사용해야 하고 감춰진 의존성을 가지면 안됨.  

ORM(Object Relational Mapping)같은 경우 데이터 레이어에 정의된 ORM 객체가 전 레이어를 돌아다닌다.