## JavaScript 기본 
https://opentutorials.org/course/743/4650

> 자바스크립트는 본래 웹 페이지를 동적으로 제어하기 위해 만들어진 언어

### 1. ECMAScript
자바스크립트 표준안, ESX 로 표현된다.

### 2. Features / Grammar
1. == 와 ===  
    - == 은 값이 같은지 비교
    - === 은 값, 타입 등이 같은지 모두 비교 
    - == 은 java 의 equals, === 은 java 의 == 이라고 볼 수 있음

2. if / while / for 등 일반적인 조건문 / 반복문 다 있음

### 3. Hoisting
> 끌어올리기, 호이스트는 변수의 정의가 범위에 따라 선언과 할당으로 분리되는 것을 말한다.  
함수 내에서 변수가 정의되었다면, 선언이 함수의 최상위로  
함수 밖 전역에서 변수가 정의되었다면, 선언이 전역 context 의 최상위로 변경된다.

1. 변수 호이스팅

    **코드로 보자**
    ~~~js
    // 1)
    // 아래의 함수는 에러가 나지 않는다.
    function() {
        console.log(x);
        var x = 100;
    }

    // 2)
    // 아래와 같이 선언과 할당이 분리되어 
    // 선언이 최상위 맥락으로 올라가는 것인 Hoist
    function() {
        var x;
        console.log(x);
        x = 100;
    }
    ~~~

    1번 함수에서는 오류가 나지 않는다 (선언을 자바스크립트 엔진이 가장 최우선으로 해석)  
    그러나 값은 100이 아니라 undefined 로 출력된다 (할당은 런타임에 되기 때문)

2. 함수 호이스팅
    > 함수 선언이 함수 실행보다 뒤에 있더라도 자바스크립트 엔진이 함수 선언을 끌어올린다.
    아래와 같은 상황에서 "foooooo" 는 정상적으로 출력된다.
    ~~~js
    foo();
    function foo() {
        console.log("foooooo");
    }
    // > foooooo
    ~~~

    > 그러나 할당의 경우에는 Hoisting 이 안된다는 것을 기억하지 
    ~~~js
    foo();
    var foo = function(){
        console.log("foooooo");
    }
    // > Syntax Error!
    ~~~

### 4. 익명함수와 IIFE (immediately-invoked function expression)

#### 익명함수 
> 이름이 없는 함수, 자바스크립트에서는 이름 없는 함수 리터럴을 변수에도 할당이 가능하다.
~~~js
const add = function (a, b) { return a + b };
~~~

그러나 아래와 같은 선언은 불가하다.  
선언부가 없으면 이를 사용할 맥락이나 이를 할당해줄 변수가 무조건 필요하다.  
*(Hoisting을 할건지, 아니면 지금 바로 쓸껀지)*
~~~js
function (a, b) { return a + b } 
// Uncaught SyntaxError: Unexpected token (
~~~

그래서 익명 함수를 쓰려면 아래 두가지 경우로 쓸 수 있다.
~~~js
// 1. 변수에 함수 리터럴 할당하기 
const add = function (a, b) { return a + b };

// 2. 즉시 사용하기 (IIFE - 아래 참조)
(function(a, b) { return a + b })(1, 2);
~~~

#### IIFE (immediately-invoked function expression) 
> 정의와 동시에 즉시 실행되는 함수 

함수를 즉시 실행시킨다.  
IIFE 의 일반적인 형태는 아래와 같다. (함수 리터럴을 괄호로 감싼 형태)
~~~js
function() {console.log("donsdev")}; // 함수 리터럴
(function() {console.log("donsdev")})(); // IIFE
~~~

### 5. 클로저 (Closure)

> 이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수를 클로져라고 한다

먼저 **일급 함수** 에 대한 이해가 필요하다.  
자바스크립트는 일급 함수를 지원하므로 함수를 변수에 저장하거나, 파라미터를 함수로 넘기고, 함수에서 함수를 반환하는 것이 가능하다.

아래를 참고하자

~~~js
var global_name = "홍길동";

function makePrinter() {
    var outer_name = "김철수";

    function printName() {
        var inner_name = "김영희";
        console.log(global_name);
        console.log(outer_name);
        console.log(inner_name);
    }

    return printName;
}

var print = makePrinter();
print();
~~~

결과
~~~
홍길동
김철수
김영희
~~~

아래 부분에서 print 변수에 할당된 것이 **클로저** 이다.  
~~~js
var print = makePrinter();
~~~

여기서 포인트는 print() 실행시, outer_name 을 그대로 가지고 출력한다는 것이다.  
따라서 함수 안에 있는 함수인 클로저는 자신을 포함하고 있는 맥락(외부함수)의 인자, 지역변수를 기억하고 있다 라는 것이다. (makePrinter() 를 했는데도 살아있다)  
이러한 변수들을 **자유 변수** 라고 한다.

클로저가 생성될 시점, 그 맥락의 지역 변수들을 자유 변수로 만드는 것을 (뭔가 그 상태 그대로 급냉하는 느낌) **캡쳐(capture)** 라고 한다. 이러한 자유변수들은 외부 접근에 의해 참조할 수 없으며, 오직 해당 closure을 이용해서만 접근할 수 있다. 객체 지향의 private 멤버 변수 같은 느낌을 가진다.

From MDN :
> 클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경(Scope)을 ‘기억한다’  

**또 다른 예**

~~~js
function getClosure() {
  var text = 'variable 1';
  return function() {
    return text;
  };
}

var closure = getClosure();
console.log(closure()); // 'variable 1'
~~~

getClosure 함수는 함수를 반환한다. 그리고 **반환된 함수**는 getClosure 의 스코프에서 선언된 변수를 참조하고 있다. 또한 참조된 변수는 함수 실행이 끝나고 반환되었다고 해서 사라지지 않았고 아래 함수의 사용에서 제대로 된 값을 반환하고 있다.

여기서 반환된 함수가 **클로저** 이다.

### 6. var, let, const 

#### 1. var
- function-scoped
    > 함수 안까지만 확장되는 스코프 
    ~~~js
    // var 변수가 호이스팅 되어 정상적으로 j가 출력된다.
    for(var j=0; j<10; j++) {
        console.log('j', j)
    }
    console.log('after loop j is ', j) 
    // after loop j is 10


    // 아래의 경우에는 function 스코프까지만 확장되어
    // 함수 밖에서 i 값을 참조할 수 없다.
    function counter () {
        for(var i=0; i<10; i++) {
            console.log('i', i)
        }
    }
    counter()
    console.log('after loop i is', i) 
    // ReferenceError: i is not defined
    ~~~

    var은 변수 재선언이 되고, 호이스팅 때문에 아래와 같은 문제가 있었다.
    ~~~js
    // 이미 만들어진 변수이름으로 재선언했는데 아무런 문제가 발생하지 않는다.
    var a = 'test'
    var a = 'test2'

    // hoisting으로 인해 ReferenceError에러가 안난다.
    c = 'test'
    var c
    ~~~

    따라서 생긴것이 let 과 const 이다.
#### 2. let / const 
- let 은 변수 재선언이 불가하고, 재할당은 가능하다.
- const 는 재선언 및 재할당 둘다 불가하다.

let, const가 hoisting이 아예 발생하지 않는것은 아니다.  
var이 function-scoped로 hoisting 이 되었다면,  
let, const는 block-scoped 단위로 hoisting이 일어난다.

~~~js
c = 'test' // ReferenceError: c is not defined
let c
~~~
위에 코드에서 ReferenceError가 발생한 이유는 호이스팅이 발생하지 않은 것이 아니라 **tdz(temporal dead zone)** 때문이다.  
또한 const 는 선언과 동시 할당이 필수이다.  

### 7. ES6 에서 추가된 기능 
https://jsdev.kr/t/es6/2944

ES6 에서 추가된 주요 기능
1. Arrows (화살표 함수)
~~~js
var evens = [2, 4, 6, 8,];

// Expression bodies (표현식의 결과가 반환됨)
var odds = evens.map(v => v + 1);   // [3, 5, 7, 9]
var nums = evens.map((v, i) => v + i);  // [2, 5, 8, 11]
var pairs = evens.map(v => ({even: v, odd: v + 1})); // [{even: 2, odd: 3}, ...]

// Statement bodies (블럭 내부를 실행만 함, 반환을 위해선 return을 명시)
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});
~~~

2. Classes  
ES6 클래스는 기존 프로토타입 기반 객체지향 패턴을 더 쉽게 사옹할 수 있도록 함

~~~js
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
~~~

3. Let, Const  
- var의 스코프는 전체 외부 함수까지이다.
- let은 변수를 선언한 블록과 그 내부 블록들에서 유효

4. Iterator / Generator  
https://infoscis.github.io/2018/01/31/ecmascript-6-iterators-and-generators/

> 많은 프로그래밍 언어는 컬렉션에서 위치를 추적하기 위해 변수가 필요한 for 루프를 사용하여 데이터를 반복하는 것에서 컬렉션의 다음 항목을 반환하는 Iterator 객체를 사용하는 방식으로 전환했다.  
Iterator를 사용하면 데이터 컬렉션을 쉽게 처리할 수 있어 ES6 에서는 Iterator를 JavaScript에 추가했습니다.  
Iterator는 새로운 Array 메서드 및 새로운 타입의 컬렉션 (Set 및 Map)과 결합하여 데이터를 효율적으로 처리할수 있는 핵심 요소이며, 이러한 부분은 JavaScript의 여러곳에서 찾아볼 수 있다.  
또한 Iterator와 함께 작동하는 새로운 for-of 루프가 있으며, Spread (...) 연산자에서도 Iterator를 사용할 수 있다.  
그리고 Iterator는 비동기 프로그래밍을 더 쉽게 만들수 있게 한다.



5. Proxy
> Spring AOP 처럼 사용 가능
~~~js
// Proxying a function object
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() // 'I am the proxy';
~~~

6. Promises
>  Promise는 미래에 생성되는 값을 나타내는 일급 객체이다.

7. Reflect API
~~~js
class Greeting {
    constructor(name) {
        this.name = name;
    }

    greet() {
      return `Hello ${name}`;
    }
}

function greetingFactory(name) {
    return Reflect.construct(Greeting, [name], Greeting);
}

greetingFactory('a'); // Greeting {name: "a"}
~~~