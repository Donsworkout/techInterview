[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. 객체 지향 프로그래밍

### 1-1) 객체 지향의 4갸지 원칙
1. **추상화 (Abstraction)**  
필요로 하는 속성이나 행동을 추출하여 추상화 시키는 것  
ex) Class / Instance 

2. **캡슐화 (Encapsulation)**  
> Message-passing의 한 부분이다.  
객체들간 메시지를 통해 소통하고 상대방의 디테일은 알 필요가 없다는 객체지향 원칙이다.

- 높은 응집도와 낮은 결합도가 객체지향의 정석  
=> 정보 은닉의 필요성 

- 클래스 변수를 public 으로 해서 다른 메소드가 직접 접근하게 한다  
=> 강한 결합, 안좋은 예시

3. **상속 (Encapsulation)**
> Generalization / Specialization

- 재사용의 관점 + 캡슐화의 일종 
- 슈퍼클래스로 자식 클래스를 외부로 부터 은닉하는 것이다. 
- in Kind of 관계가 성립되지 않으면 불필요한 속성이나 연산을 물려받게 된다.

4. **다형성**
> 서로 다른 클래스의 객체가 같은 메시지를 받았을때 각자의 방식으로 동작하는 능력이다.
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

### 싱글턴 (Singleton)
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

### Strategy 패턴  
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