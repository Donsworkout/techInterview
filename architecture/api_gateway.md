[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)


## API gateway
> 앞단에서, 여러 API 서버의 end-point를 단일화 해 주는 것  
<img src="https://t1.daumcdn.net/cfile/tistory/9912B13A5A900B5310"/>  

### API gateway의 장점
1. 여러 마이크로서비스 라우팅
> 각각 서비스마다 호출해줄 주소를 통일해 준다. 

2. 보안
> API gateway의 주소는 public 으로 열어두고, 각 서비스들은 private 으로 외부에서의 접근은 막는다.

3. 각 서비스의 공통 로직을 처리해 준다  
> 로깅, CORS, 모니터링, 인증 등 공통 기능을 API Gateway의 필터 기능으로 해결할 수 있다.
