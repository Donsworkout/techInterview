[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. 객체 지향 프로그래밍

### 1-1) 객체 지향의 4갸지 원칙
1. **추상화 (Abstraction)**  
필요로 하는 속성이나 행동을 추출하여 추상화 시키는 것  
ex) Class / Instance 

2. **캡슐화 (Encapsulation)**  
> Message-passing의 한 부분이다, 접근 지정자의 형태로 나타남 (private, protected)  
객체들간 메시지를 통해 소통하고 상대방의 디테일은 알 필요가 없다는 객체지향 원칙이다.

  캡슐화의 장점
  1. 낮은 결합도  
  객체간 소통시 세부사항 알 필요 없음

  2. 정보 은닉   
  정보 손상 가능성 낮춤 (public -> private)

3. **상속 (Encapsulation)**
> Generalization / Specialization

- 재사용의 관점 + 캡슐화의 일종 
- 슈퍼클래스로 자식 클래스를 외부로 부터 은닉하는 것이다. 
- in Kind of 관계가 성립되지 않으면 불필요한 속성이나 연산을 물려받게 된다.

4. **다형성**
> 여러 가지 형태를 가질 수 있는 능력  
자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록 함

- 오버로딩 / 오버라이딩  
- 자바에서 인터페이스 인스턴스에 구현체를 할당하는 경우
- 자바에서 하위 클래스를 상위 클래스의 데이터 타입으로 인스턴스화하는 경우

~~~java
// 부모 클래스
public abstract class Pet {
  public abstract void talk();
}
// 자식 클래스
public class Cat extends Pet {
  public void talk(){ System.out.println("야옹"); }
}
public class Dog extends Pet {
  public void talk(){ System.out.println("멍멍"); }
}
public class Parrot extends Pet {
  public void talk(){ System.out.println("안녕"); }
}
~~~ 

4-1) 오버라이딩 (Overriding)  
- 슈퍼클래스에서 정의한 함수를 자식 클래스에서 덮어씌우는 것
- 자바의 abstract 함수가 그 예
- 즉 클래스 외적인 개념 

4-2) 오버로딩 
- 같은 클래스 내에서, 메소드 이름은 같지만 시그니처 (파라미터 유형 갯수 등) 이 다름


## 2. 디자인 패턴 
> 바퀴를 다시 발명하지 마라,  
각기 다른 SW가 서로간의 공통되는 설계 문제가 존재하며 이를 해결하는 방법론 

### GoF (Gang of Fouts) 디자인 패턴의 분류  
1. 생성 (Creational Pattern) 패턴
- 객체 생성에 관련된 패턴
- 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.
- 싱글턴이 생성 패턴에 포함 

2. 구조 (Structural) 패턴
- 클래스나 객체를 조합해 더 큰 구조를 만듬

3. 행위 (Behavior) 패턴
- 객체나 클래스 사이의 책임 분배와 관련된 패턴
- 한 객체가 혼자 수행할 수 없는 작업을 여러개의 객체로 어떻게 분배하는지. 그렇게 하면서도 객체간 결합도는 최소화 한다. 
- Strategy(전략패턴) 가 여기에 포함 

### 싱글턴 패턴 (Singleton)
- 전역 변수를 쓰지 않고, 객체를 하나만 생성하게 한다.
- getInstance 형태로 프로그램 어디에서나 끌어쓸 수 있다.
- 스레드 상황에서 문제가 발생할 수 있는데 이는 static 변수로 바로 멤버변수에 할당하면 된다. 

~~~java
public class Printer {
   // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
   private static Printer printer = new Printer();
   private Printer() { }
   // 자기 자신의 인스턴스를 외부에 제공
   public static Printer getPrinter(){
     return printer;
   }
   public void print(String str) {
     System.out.println(str);
   }
}
~~~

### 전략 패턴 (Strategy) 
- 상황에 따라, 비슷하면서도 조금씩 다른 행위을 주고 싶을때 사용한다.
- 인터페이스는 하나요, 인터페이스 생성될떄 그 구현체를 선택하는 형태이다.
- 인터페이스를 전략 인터페이스라고 부르고,
- 구현체들을 전략 클래스라고 부른다.
- 구현체들은 같은 이름의 함수를 가지고 있고 액션은 다르다.

먼저 전략 인터페이스는 아래와 같이 구현체의 공통 메솓드로!
~~~java
public interface Strategy {
    void runStrategy();
}
~~~

전략 인터페이스를 구현한 클래스는 아래와 같다.
~~~java
public class StrategyGun implements Strategy{
    @Override
    public void runStrategy() {
        // TODO Auto-generated method stub
        System.out.println("탕! 타탕! 탕탕!");  
    }
}
~~~

전략을 사용할 컨텍스트가 필요하고  
~~~java
public class Solider {
    void runContext(Strategy strategy) {
        System.out.println("배틀 그라운드 시작");
        strategy.runStrategy();
        System.out.println("배틀 종료");
    }
 
}
~~~
그리고 전략을 생성하고 컨텍스트에 주입니다. 
~~~java 
public class Client {
    public static void main(String[] args) {
        Strategy strategy = null;
        Solider rambo = new Solider();
        // 인터페이스에 생명력 부여
        strategy = new StrategyGun();
         
        rambo.runContext(strategy);
         
        System.out.println("\n");
        strategy = new StrategyGrenade();
         
        rambo.runContext(strategy);
    }
}
~~~

## 3. 객체 지향의 5대 원칙 (SOLID)
> High Cohesion - Loose Coupling 원칙을 객체 지향애 도입한 것, 재사용은 극대화 시키되 수정은 최소화 하는것이 목표 

### 1. 객체지향 5원칙
1. SRP (Single Responsibility Principle) 단일 책임 원칙  
2. OCP (Open Closed Principle) 개방 폐쇄 원칙
3. LSP (Liskov Substitution Principle) 리스코프 치환 원칙
4. ISP (Interface Segregation Principle) 인터페이스 분리 원칙
5. DIP (Dependency Inversion Principle) 의존 역전 원칙

### 1.1 객체지향 5원칙 상세
#### 1. SRP (Single Responsibility Principle)  
**단일 책임 원칙**
  > 클래스의 역할과 책임을 명확하게 하고, 추상화의 정도를 지켜야 한다.

#### 2, OCP (Open Closed Principle)  
**개방 폐쇄 원칙**
  > 소프트웨어는 확장에는 열려있고, 주변의 변화에 대해서는 닫혀 있어야 한다.

기존 제휴사와의 서비스에서 비슷하게 다른 제휴사의 서비스를 추가할 때, 기존 코드의 수정은 불가피하지만, 최대한 프로그램의 수정은 줄이되 확장은 용이하도록 설계해야 한다.  
이와 관련하여 적절한 예는, **인터페이스** 이다.

![99B9E433599E5A9E07](https://user-images.githubusercontent.com/26560119/64069260-a6524380-cbe1-11e9-9114-0cd5fc828605.png)

인터페이스나 상위 클래스 (상속) 은 세부 구현과 다른 모듈의 의존성을 약화시키는 완충장치이다.

#### 3. LSP (Liskov Substitution Principle)   
**리스코프 치환 원칙** 
> 서브 타입은 언제나 자신의 기반 타입(base type)으로 교체할 수 있어야 한다.  
(의미론적으로 sub type ISA base type 이 되어야 한다는 뜻)  

LSP(리스코프 치환 원칙)은 인터페이스와 클래스 관계, 상위 클래스와 하위 클래스 관계를 얼마나 잘 논리적으로 설계 했느냐가 관건이다. (상속을 제대로 구현했는지)  
그리고 **상속 == 확장** 이라는 것을 기억하자.

#### 4. ISP (Interface Segregation Principle)  
**인터페이스 분리 원칙**

> 클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안된다.  
(상황과 관련있는 메서드만 지원해야 한다.)

ISP(인터페이스 분리 원칙)은 SRP(단일 책임 원칙)과 다른 해결책을 제시하는 것이다. 

SRP(단일 책임 원칙) => 클래스를 쪼개서 기능별로 클래스 자체를 분리

ISP(인터페이스 분리 원칙) => 인터페이스 정제 및 최소화



#### 5. DIP (Dependency Inversion Principle)  
**의존 역전 원칙**

"고차원 모듈은 저차원 모듈에 의존하면 안된다. 이 두 모듈 모두 다른 추상화 된 것에 의존해야 한다." 
"추상화 된 것은 구체적인 것에 의존하면 안된다. 구체적인 것이 추상화 된 것에 의존해야 한다."
"자주 변경되는 구체(Concrete) 클래스에 의존하지 마라" - 로버트 C. 마틴

> 자신보다 변하기 쉬운 것에 의존하지 마라.

구체적으로 추상클래스 또는 상위클래스는 구체적인 구현클래스 또는 하위클래스에게 의존적이면 안된다. 구체 클래스는 자주 변경되기 때문에 이에 의존하게 되면 의존하는 고차원 클래스들을 모두 수정해야 한다. 지양하자