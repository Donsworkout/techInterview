[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Data Structures 
### 1. Lists.newArrayList
> 구아바 빌더는 유형 인수를 여러 번 입력하는 것을 절약한다.
~~~java
List<Foo<Bar, Baz>> list = Lists.newArrayList();
List<Foo<Bar, Baz>> list = new ArrayList<Foo<Bar, Baz>>();
~~~
> 다이아몬드 연산자랑 같다고 보면 된다.
~~~java
List<Foo<Bar, Baz>> list = new ArrayList<>();
~~~

### 2. 자바 제네릭 - 다이아몬드 연산자 (Java 7 부터 지원)
> cpp 의 템플릿과 비슷함  

java7 이전에는 아래와 같이 선언 후 사용했다.  

~~~java
//Case 1. 
List<Integer> list = new LinkedList(); 
//Case 2. 
List<Integer> list = new LinkedList<Integer>();
~~~

case 1 같이 쓰면 컴파일 에러는 안나지만, 런타임 에러가 발생하고,  
case 2 는 안전하지만, java 7 부터는 아래와 같이 작성이 가능 해 졌다.
~~~java
List<Integer> list = new LinkedList<>();
~~~
이렇게 다이아몬드를 쓰면, 굳이 뒤에 자료형을 명시할 필요가 없다.

### 3. 자바 Optional
https://m.blog.naver.com/PostView.nhn?blogId=hehe5959&logNo=221002524184&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F
> 존재할 수도 있지만 안할 수 도 있는 객체 (nullable 한 객체를 wrapping하고 있음)  
제너릭도 사용가능하다.
~~~java
Optional<Order> maybeOrder;  
// Order 타입의 객체를 감쌀 수 있는 Optional 타입의 변수
Optional<Member> optMember; 
// Member 타입의 객체를 감쌀 수 있는 Optional 타입의 변수
Optional<Address> address; 
// Address 타입의 객체를 감쌀 수 있는 Optional 타입의 변수
~~~

### 4. Map 과 MultiValueMap 은 무엇이 다를까?
**A MultivaluedMap is:**  
A map of key-values pairs. Each key can have zero or more values.

**A Map is:**   
An object that maps keys to values. A map cannot contain duplicate keys; each key can map to at most one value.

> 즉 MultiValueMap 은 한 key에 여러 value가 가능하고  
Map은 한 value만 가능하다.

중요한 특징중에 하나는 restTemplate의 exchange 메서드의 세번째 파라미터 request에는 무조건 MultiValueMap을 사용해야 한다. 

~~~java
MultiValueMap<String, String> map= new LinkedMultiValueMap<>();
map.add("phone", "01087578935");
map.add("name", "김도현");

HttpEntity<MultiValueMap<String, String>> request = new HttpEntity<>(map, headers);

ResponseEntity<List<UserInfoDTO>> apiResponse = restTemplate.exchange(URL, HttpMethod.POST, request, new ParameterizedTypeReference<List<UserInfoDTO>>() {});
return apiResponse.getBody();
~~~

### 5. 배열과 리스트의 차이 
### Array
> 일반적으로 배열은 크기가 확정되어 있고 인덱스가 중요할 때, 효율이 좋아 많이 쓰인다.
- 인덱스를 활용하여 빠른 조회가 가능하다.
- 물리적으로 인접해 있어 cache hit (공간 지역성) 의 가능성이 높아진다.
- 대신 리스트에 비해 기능이 없다.
- 가변 배열을 쓰면 기존 배열 그대로 두고 새로운 길이로 배열 할당 후 데이터 복사하고 기존 배열을 삭제한다.
- 요소 삭제시 메모리가 낭비된다.

### List 
> 인덱스가 중요하지 않을떄 빈틈없음 데이터의 적재가 가능
- 순서가 있는 데이터의 모임
- 배열의 index가 그 배열의 Key 라면, 리스트의 index 는 순서라는 의미 정도이다.
- 데이터 추가 / 삭제가 많이 일어난다면 리스트가 적합하다.
