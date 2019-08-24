[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## Service Discovery
> 보통 대용량 트래픽이 예상되는 서버에서는 가용성을 위해  
하나의 인스턴스에 하나의 서비스를 올려두는 것이 아니라  
두개 이상의 인스턴스에 동일한 서비스를 올린 후, 한쪽에서 문제가 발생하면 다른 인스턴스를 통해 로직을 수행한다.

이처럼 클라우드 환경의 Auto Scaling 이나, 확장 등의 이유로  
인스턴스가 생성, 소멸 될 시 IP 주소가 변경되기도 한다.  
따라서 비즈니스 로직은 이 상태를 알 수 없다.  
만약 Auto Scaling으로 인스턴스가 하나 생성되었더라도  
로직은 이를 스스로 알아차리고 생성된 인스턴스에 트래픽을 분산시킬 수 없다.  

따라서 이러한 변화를 감지하는 역할이 **Service Discovery** 다.

Service Discovery가 어떻게 진행되는지 살펴 본다면 아래와 같다.
<img src="https://t1.daumcdn.net/cfile/tistory/99E44A3A5A955A902C"/>  
1. 각 서비스가 실행(호출) 될 때, Service Registry에 IP,port 등의 서버 정보를 저장한다.
2. Client에서 HTTP 요청이 들어온다.
3. Router은 정해진 시간마다 주기적으로 Service Registry에 가서 서비스들의 서버 정보를 가져온다.
4. 요청이 들어온 URL을 해석해, Service Registry에서 가져온 서버 정보를 토대로 알맞는 서버로 라우팅을 시킴

> 요청할때마다 Service Registry를 통해 서버 정보를 가져오지는 않고, 캐싱을 이용함  
그리고 Service Registry는 주기적으로 인스턴스에 ping를 보내서 서버의 생존여부를 파악하고,  
죽어있으면 Service Registry에서 정보를 지운다.  

그리고 Service Registry에서 IP, PORT를 이미 가지고 있기 때문에  
요청이 들어올 시, DNS 서버에 다녀오지 않아서 성능상에 이점이 있다.
