[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 1. OSI7 계층 
> 컴퓨터 네트워크 통신과 프로토콜을 계층으로 나누어 설명한 것

![osi-7-layer](https://user-images.githubusercontent.com/26560119/64315569-60381e00-cfed-11e9-8ba3-c56d84afc151.png)

- 각 계층은 하위 계층의 기능만을 이용하고, 상위 계층에게 기능을 제공한다.
- 일반적으로 하위 계층들은 하드웨어로, 상위 계층들은 소프트웨어로 구현된다.
- 네트워크 관리자는 어떤 문제의 원인이 어디에 있는지 범위를 좁힐 수 있다
- 언제나 그렇듯 역할과 책임의 분리 (추상화)
- 각 계층의 전달 단위는 프레임, 패킷, 세그먼트지만 데이터는 같고 헤더 정보만 다르다 (MAC, IP, PORT 등)

1. 물리 계층 (Physical Layer)  
    - 시스템의 전기적 물리적 표현을 나타낸다.  
    - 구분 주소는 MAC
    - 네트워크 케이블, 주파수 등 연결과 관련된 HW 적인 Feature 이다.
    - 전송 단위는 Bit 이다

2. 데이터 링크 계층 (Data-link Layer)
    - 노드간 데이터 전송을 나타낸다.
    - CRC 기반의 오류, 흐름 제어가 들어간다. (약하게)
    - 구분 주소는 MAC
    - Ethernet (통신 기술 표준)
        - 저비용 / 비 연결성, 비 신뢰성
    - 데이터 전송 단위는 Frame

3. 네트워크 계층 (Network Layers)
    - MAC 주소 가지고는 통신불가, 상위 레이어가 필요
    - 여러개의 노드를 거칠때마다 경로를 찾아주는 역할을 하는 계층 (Routing)
    - 라우팅, 흐름 제어, 세그멘테이션(segmentation/desegmentation), 오류 제어, 인터네트워킹(Internetworking)
    - 논리적인 주소 구조(IP), 곧 네트워크 관리자가 직접 주소를 할당하는 구조를 가짐
    - 데이터 전송 단위는 Datagram(Packet) 이다.

4. 전송 계층 (Transport Layer)
    - End to End 사용자간 데이터 전달 신뢰성을 보장해 주는 Layer
    - 시퀀스 넘버 기반의 오류제어 
    - 전송 계층은 특정 연결의 유효성을 제어하고, 일부 프로토콜은 상태 개념이 있고(stateful), 연결 기반(connection oriented)이다. (이는 전송 계층이 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송한다는 것을 뜻한다.)
    - 가장 잘 알려진 전송 계층의 예는 TCP이다.
    - 데이터 전송 단위는 Segment이다.
    - PORT 로 프로세스를 구분한다. 

5. 세션 계층 (Session Layer)
    - 세션 연결의 설정과 해제, 세션 메시지 전송 등의 기능을 한다.
    - 성립된 세션을 통한 상호 대화 관리를 하는 양단간 응용 개체를 위해 토큰 개념이 정의
    - 세션계층은 전송시 동기점을 삽입함으로써 메세지를 대화 단위로 그룹화 함  
    에러 발생하면 중단된 대화 단위의 처음부터 전송을 다시 시작

6. 표현 계층 (Presentation Layer)
    - 여러 다른 회사의 네트워크 시스템마다 다른 데이터 표현 방식을 사용하는데 그것을 통일하는 계층임 (암호화, 압축)
    - JPEG, TIFF, GIF(그래픽 포맷), MPEG, QUICKTIME(동영상 포맷), MIDI(음악 포맷), RTF, ASCII, EBCDIC(텍스트 포맷)등을 지정 한다

7. 응용 계층 (Application Layer)
    - 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.
    - 사용자가 직접 눈으로 보고 실제로 작업을 하는 계층이다. 
    - 웹 브라우저, HTTP, FTP, WWW, Telnet, SMTP, POP 

## 2. TCP / UDP
> 네트워크 계층 중 전송(Transport) 계층에서 사용하는 프로토콜

### 1. TCP
**신뢰성** 보장이 아주 중요한 키워드이다.  

<img width="326" alt="tcp-virtual-circuit" src="https://user-images.githubusercontent.com/26560119/64318280-ad6bbe00-cff4-11e9-8292-b3115ca400a6.png">

- 인터넷 상에서 세그먼트를 보내기 위해 IP와 함께 사용하는 프로토콜이다.
- TCP와 IP를 함께 사용하는데, IP가 주소를 찾아주면 TCP는 흐름 제어, 혼잡 제어 등 신뢰성을 보장한다. 
    - 흐름제어 (**Host to Host** / 송신측이 수신측보다 과하게 빠른 문제 해결)
        - 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것
        - 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.
    - 혼잡제어 (**Host to Network** / **네트워크**의 과부하 조절)
        - 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것
        - 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다
- **연결형 서비스이다**
    - 3-way handshaking과정을 통해 연결을 설정하고, 4-way handshaking을 통해 연결을 해제한다.
    - https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#tcp%EC%9D%98-3-way-handshake%EC%99%80-4-way-handshake 
- 전이중(Full-Duplex), 점대점(Point to Point) 방식이다.
    - 전이중
        - 전송이 양방향으로 동시에 일어날 수 있다.
    - 점대점
        - 각 연결이 정확히 2개의 종단점을 가지고 있다.
- UDP보다 속도가 느리다.

### 2. UDP
**비연결형 프로토콜** 이라는 점에 주목

<img width="326" alt="udp-datagram" src="https://user-images.githubusercontent.com/26560119/64318817-f5d7ab80-cff5-11e9-8c72-c7e7f3019c6d.png">
- 4계층 프로토콜이긴 한데 IP만 보고 움직임 
- 패킷 단위로 전송
- TCP 처럼 연결형이 아니라, 데이터 순서 보장 X (지맘대로 감)
- 그대신 빠름 (스트리밍에 적합)
- UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
- 흐름제어(flow control)가 없어서 패킷이 제대로 전송되었는지, 오류가 없는지 확인할 수 없다. (성능 중시)

## 3. 3-way handshake / 4-way handshake
### 1. 3-way handshake (연결 시작)

![222D873E5815FD142E](https://user-images.githubusercontent.com/26560119/64319805-359f9280-cff8-11e9-970a-b95c6d6bff14.jpeg)

1. A : SYN (포트 열어라 새끼야)
2. B : ACK (알겠어), SYN (근데 너도 포트 열어)
3. A : ACK (OK 나도 열었어)

### 2. 4-way handshake  (연결 종료)

![2301313E5815FF5A27](https://user-images.githubusercontent.com/26560119/64319806-359f9280-cff8-11e9-8967-3e58713cb604.png)

1. A : FIN (얀결 끊자)
2. B : ACK (OK, 끊자는 메시지 받았어) ... 일 처리하던거 마무리
3. A : ... 대기중
4. B " FIN (나 연결 끊었어)
5. A : ACK (OK 나도 좀만 기다리다 끊을게)
6. A : TIME WAIT

## 4. HTTP
> HTTP => Hyper Text Transfer Protocol  
인터넷에서 데이터를 주고받을 수 있는 프로토콜,  
웹 서버와 클라이언트 간의 통신을 하기 위한 통신 규약

### 1. 주요 특징
- HTTP 는 웹에서만 사용하는 프로토콜이며, TCP/IP 기반이다.
- HTTP는 비연결성 프로토콜이다  
- 비연결성의 단점보완을 위해 Cookie와 Seesion 등장
- 응답은 html 뿐 아니라 json, xml 등으로도 받을  수 있다.

### 2. HTTP Method
1. GET
    - 데이터가 URL에 노출된다.
        ~~~
        www.urladdress.xyz?name1=value1&name2=value2
        ~~~
    - **무언가를 가져오는 요청 (select), 상태변화 X**
    - HTTP 패킷의 Body는 비어 있는 상태로 전송한다.  
        즉, Body의 데이터 타입을 표현하는 'Content-Type' 필드도 HTTP Request Header에 들어가지 않는다.
    - 브라우저 캐싱을 지원하여 빠르다 

2. POST
    - 요청 정보를 HTTP 패킷의 Body 안에 숨겨서 서버로 전송한다.
    - **서버의 값이나 상태를 바꾸기 위한** 용도의 메서드
    - Insert, Update, Delete
    - Request Header의 Content-Type에 해당 데이터 타입이 표현됨
    application/octet-stream , text/plain, multipart/form-data 등
    
3. HEAD 
    - GET 과 동일한데, HTTP 헤더 정보만 요청 
    - 네트워크 대역 절약

4. PUT
    - 요청된 자원을 수정(UPDATE)한다. 
    - 자원내 모든 필드영역 업데이트 (DB 1 ROW 전체 수정)
    - 만약 일부만 전달할 경우, 그외의 필드 모두 null or 초기값 처리
    - 따라서 PUT 잘못쓰면 골로간다
    - 보안 위협

5. PATCH
    - PUT과 유사하게 요청된 자원을 수정(UPDATE)할 때 사용한다. 
    - PUT의 경우 자원 전체를 갱신하는 의미지만, PATCH는 해당자원의 일부를 교체하는 의미로 사용.

6. DELETE
    - 자원 삭제 
    - 보안 위협

7. OPTIONS
    - 요청한 URL이 어떤 메서드를 제공하는지 체크 
    - 보안 위협

8. TRACE
    - 받은 메시지를 다시 돌려보낸다.

9. CONNECT
    - 프록시에 사용하기 위해 예약된 메서드이다
        - 프록시 서버의 기본적인 동작은 클라이언트로부터 전달받은 리퀘스트를 서버로 전달하고, 서버에서 받은 응답을 클라이언트에 전달하는 역할을 한다.
        - 프록시의 사용 목적은 네트워크 캐싱, 조직내의 특정 웹사이트 액세스 방지, 액세스 로그 획득 등이 있다.

### 3. HTTP Request / Response 헤더
- HTTP 헤더 - 일반 헤더 (General Header) 
    - 요청 및 응답 메시지 모두에서 사용 가능한 일반 목적의(기본적인) 헤더 항목
    - 주요 항목
        - **Date**: HTTP 메시지를 생성한 일시 
            - `Date: Sat, 2 Oct 2018 02:00:12 GMT`
        - **Connection**: 클라이언트와 서버 간 연결에 대한 옵션 설정
            - `Connection: close` => 현재 HTTP 메시지 직후에 TCP 접속을 끊는다는 것을 알림
            - `Connection: Keep-Alive` => 현재 TCP 커넥션을 유지
        - **Cache-Control** 
            - `Cache-Control: no-store` => 캐시 안쓴다
            - `Cache-Control: no-cache` =>  모든 캐시를 쓰기 전에 서버에 이 캐시 진짜 써도 되냐 물어보라는 뜻  
            ... 이 외에도 몇가지 있음
        - **Pragma**
            - http 1.0 레거시, 캐시 컨트롤이랑 같음 
        - **Trailer**
            - http 헤더 뒤에 임의 추가 가능
                ~~~
                HTTP/1.1 200 OK 
                Content-Type: text/plain 
                Transfer-Encoding: chunked
                Trailer: Expires
                7\r\n 
                Mozilla\r\n 
                9\r\n 
                Developer\r\n 
                7\r\n 
                Network\r\n 
                0\r\n 
                Expires: Wed, 21 Oct 2015 07:28:00 GMT\r\n
                \r\n
                ~~~
-  HTTP 헤더 - 엔터티 헤더 (Entity Header) 
    - 요청 및 응답 메시지 모두에서 사용 가능한 Entity(콘텐츠, 본문, 리소스 등)에 대한 설명 헤더 
    - 주요 항목들
        - **Content-Type**: 해당 개체에 포함되는 미디어 타입 정보
            - 컨텐츠의 타입(MIME 미디어 타입) 및 문자 인코딩 방식(EUC-KR,UTF-8 등)을 지정
            - 타입 및 서브타입(type/subtype)으로 구성 
            - `Content-Type: text/html; charset-latin-1` => 해당 개체가 html으로 표현된 텍스트 문서이고, iso-latin-1 문자 인코딩 방식으로 표현됨
        - **Content-Language**: 해당 개체와 가장 잘 어울리는 사용자 언어(자연언어)
        - **Content-Encoding**: 해당 개체 데이터의 압축 방식
            - `Content-Encoding: gzip, deflate`
            - 만일 압축이 시행되었다면, Content-Encoding 및 Content-Length 2개 항목을 토대로 압축 해제 가능 
        - **Content-Length**: 전달되는 해당 개체의 바이트 길이 또는 크기(10진수)
        - **Content-Location**: 해당 개체가 실제 어디에 위치하는가를 알려줌
        - **Content-Disposition**: 응답 Body를 브라우저가 어떻게 표시해야 할지 알려주는 헤더
            - inline인 경우 웹페이지 화면에 표시되고, attachment인 경우 다운로드
            - `Content-Disposition: inline`
            - `Content-Disposition: attachment; filename='filename.csv'`
        - **Content-Security-Policy**: 다른 외부 파일들을 불러오는 경우, 차단할 소스와 불러올 소스를 명시
            - *XSS 공격*에 대한 방어 가능 (허용한 외부 소스만 지정 가능)
            - `Content-Security-Policy: default-src https:` => https를 통해서만 파일을 가져옴
            - `Content-Security-Policy: default-src 'self'` => 자신의 도메인의 파일들만 가져옴
            - `Content-Security-Policy: default-src 'none'` => 파일을 가져올 수 없음
        - **Location**: 리소스가 리다이렉트(redirect)된 때에 이동된 주소, 또는 새로 생성된 리소스 주소
            - 300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려주는 헤더
            - 새로 생성된 경우에 HTTP 상태 코드 `201 Created`가 반환됨
            - `HTTP/1.1 302 Found  Location: /`
                - 이런 응답이 왔다면 브라우저는 / 주소로 redirect한다.
        - **Last-Modified**: 리소스를 마지막으로 갱신한 일시
- HTTP 헤더 내 요청 헤더 (Request Header) 항목
    - 요청 헤더는 HTTP 요청 메시지 내에서만 나타나며 가장 방대하다.
    - 주요 항목들
        - **Host**: 요청하는 호스트에 대한 호스트명 및 포트번호 (***필수***)
            - Host 필드에 도메인명 및 호스트명 모두를 포함한 전체 URI(FQDN) 지정 필요
            - 이에 따라 동일 IP 주소를 갖는 단일 서버에 여러 사이트가 구축 가능
        - **User-Agent**: 클라이언트 소프트웨어(브라우저, OS) 명칭 및 버전 정보
        - **From**: 클라이언트 사용자 메일 주소 
            - 주로 검색엔진 웹 로봇의 연락처 메일 주소를 나타냄
            - 때로는, 이 연락처 메일 주소를 User-Agent 항목에 두는 경우도 있음
        - **Cookie**: 서버에 의해 Set-Cookie로 클라이언트에게 설정된 쿠키 정보
        - **Referer**: 바로 직전에 머물었던 웹 링크 주소
        - **If-Modified-Since**: 제시한 일시 이후로만 변경된 리소스를 취득 요청
        - **Authorization**: 인증 토큰(JWT/Bearer 토큰)을 서버로 보낼 때 사용하는 헤더
            - 토큰의 종류(Basic, Bearer 등) + 실제 토큰 문자를 전송
        - **Origin**
            - POST 요청 주소
            - 여기서 요청을 보낸 주소와 받는 주소가 다르면 **CORS 에러** 가 발생
            - 응답 헤더의 **Access-Control-Allow-Origin**와 관련 
    - 다음 4개는 주로 HTTP 메세지 Body의 속성 또는 내용 협상용 항목들
        - **Accept**: 클라이언트 자신이 원하는 미디어 타입 및 우선순위를 알림
            - `Accept: */*` => 어떤 미디어 타입도 가능
            - `Accept: image/*` => 모든 이미지 유형
        - **Accept-Charset**: 클라이언트 자신이 원하는 문자 집합
        - **Accept-Encoding**: 클라이언트 자신이 원하는 문자 인코딩 방식
        - **Accept-Language**: 클라이언트 자신이 원하는 가능한 언어
        - 각각이 HTTP Entity Header 항목 중에 `Content-Type, Content-Type charset-xxx, Content-Encoding, Content-Language`과 일대일로 대응됨
- HTTP 헤더 내 응답 헤더 (Response Header) 항목
    - 특정 유형의 HTTP 요청이나 특정 HTTP 헤더를 수신했을 때, 이에 응답한다.
    - 주요 항목들
        - **Server**: 서버 소프트웨어 정보
        - **Accept-Range**
        - **Set-Cookie**: 서버측에서 클라이언트에게 세션 쿠키 정보를 설정 (RFC 2965에서 규정)
        - **Expires**: 리소스가 지정된 일시까지 캐시로써 유효함
        - **Age**: 캐시 응답. max-age 시간 내에서 얼마나 흘렀는지 알려줌(초 단위)
        - **ETag**: HTTP 컨텐츠가 바뀌었는지를 검사할 수 있는 태그
        - **Proxy-authenticate**
        - **Allow**: 해당 엔터티에 대해 서버 측에서 지원 가능한 HTTP 메소드 리스트
            - 때론, HTTP 요청 메세지의 HTTP 메소드 OPTIONS에 대한 응답용 항목
                - OPTIONS: 웹서버측 제공 HTTP 메소드에 대한 질의
            - `Allow: GET,HEAD` => 웹 서버측이 제공 가능한 HTTP 메서드는 GET,HEAD 뿐임을 알림 (405 Method Not Allowed 에러와 함께)
        - **Access-Control-Allow-Origin**: 요청을 보내는 프론트 주소와 받는 백엔드 주소가 다르면 *CORS 에러*가 발생
            * 서버에서 이 헤더에 프론트 주소를 적어주어야 에러가 나지 않는다.
            * `Access-Control-Allow-Origin: www.zerocho.com`
                * 프로토콜, 서브도메인, 도메인, 포트 중 하나만 달라도 CORS 에러가 난다. 
            * `Access-Control-Allow-Origin: *`
                * 보안 박살
            * 유사한 헤더로 `Access-Control-Request-Method, Access-Control-Request-Headers, Access-Control-Allow-Methods, Access-Control-Allow-Headers` 등이 있다. 
### 4. HTTP Keep-Alive
HTTP 는 비연결 방식으로 연결을 매번 만들고 끊는 구조이다.  
이는 많은 비용이 드므로, HTTP 1.1 부터는 Keep-Alive (연결 유지) 를 제공한다.

- HTTP/1.1부터 지원하는 기능으로 TCP 연결을 재사용하는 기능이다.
- Handshake 과정이 생략되므로 성능 향상을 기대 할 수 있다 (TCP 소켓 세션 유지)
-  모든 TCP 세션을 무한정 유지할 수는 없으므로 Timeout 및 Max 설정을 통해 관리되어야 한다.
- 연결된 Socket에 IO 의 Access가 마지막으로 종료된 시점부터 
정의된 시간까지 Access가 없더라도 세션을 유지하는 구조이다.
- 정의된 시간내에 Access가 이루어진다면 계속 연결된 상태를 유지할 수 있게 된다.

### 5. HTTP Status Code

#### 100 - 199 : 정보성 상태 코드

- 100 : 요청의 시작 부분 일부가 받아들여졌으며, 클라이언트는 나머지를 계속 이어서 보내야 함을 의미함

- 101 : 요청자가 서버에 프로토콜 전환을 요청했으며, 서버에서 이를 승인하는 중을 의미함

#### 200 - 299 : 성공 상태 코드
클라이언트가 요청을 보내면, 요청은 대게 성공함. 서버는 대응하는 성공을 의미하는 각각의 요청에 대응한다.

- 200 : 요청은 정상이고, 본문은 요청된 리소스를 포함하고 있다.

- 201 : 어떠한 생성 작업을 요청받았으며, 생성 작업을 성공하였다.

- 202 : 요청은 받아들여졌으나, 아직 동작을 수행하지 않은 상태로 요청이 적절함을 의미함

- 203 : 요청을 성공하였지만, 요청에 대한 검증이 되지 않은 상태를 의미함

- 204 : 요청을 성공하였지만, 제공할 내용이 없음을 의미

- 205 : 204와 동일하지만 새로고침등을 통해 새로운 내용등을 확인할것을 추가로 의미

- 206 : 요청의 일부분만 성공하였음을 의미

#### 300 - 399 : 리다이렉션 상태 코드
클라이언트에 요청에 대해 적절한 다른 위치를 제공하거나, 대안의 응답을 제공한다.

- 300 : 클라이언트가 동시에 여러 응답을 가리키는 URL을 요청한 경우 응답 목록과 함께 반환된다.(ex] 어떠한 HTML에 문서에 대한 영문페이지와 불어페이지를 요청)

- 301 : 요청한 URL이 옮겨졌을 때 사용. 옮겨진 URL에 대한 정보와 함께 응답되어야 한다.

- 302 : 301과 동일하나, 클라이언트는 여전히 옮겨지기전 URL로 요청할것을 의미

- 303 : 요청받은 행동 수행을 위해서는 다른 URL로 요청 해야함을 의미

- 304 : 이전의 동일한 요청과 비교하여 변화가 없음을 의미 (단시간에 반복된 동일 요청에 대한 대응 코드)

- 305 : 직접적인 요청이 아니라 반드시 프락시(우회경로)를 통해 요청되어야 함을 의미

- 307 : 302와 동일하며, HTTP Method도 변경없이 요청하여야 함을 의미

#### 400 - 499 : 클라이언트 에러 상태 코드
클라이언트의 잘못된 요청에 대한 대응 코드

- 400 : 클라이언트가 올바르지 못한 요청을 보내고 있음을 의미

- 401 : 요청을 위해서는 권한 인증등을 요구함을 의미 (NOT AUTHORIZED)

- 403 : 요청이 서버에 의해 거부 되었음을 의미 (FORBIDDEN)

- 404 : 요청한 URL을 찾을 수 없음을 의미 (NOT FOUND)

- 405 : 요청한 URL이 Method를 지원하지 않음을 의미 (ex] POST요청에 대한 응답을 하는 URL에 GET으로 요청) (NOT ALLOWED)

- 406 : 클라이언트 요청에 대해 적절한 컨텐츠가 없음을 의미

- 407 : 401과 동일하나, 프락시(우회경로)를 통하여 인증 할 것을 요구함을 의미

- 408 : 요청에 응답하는 시간이 너무 많은 시간이 걸림을 의미(서버는 요청을 끊을수 있음)

- 409 : 클라이언트 요청에 대해 서버에서 충돌 요소가 발생 할수 있음을 의미

- 410 : 요청한 URL이 더 이상 사용되지 않고 사라졌음을 의미

- 411 : 클라이언트 요청에 Content-length 헤더가 포함되어야 함을 의미

- 412 : 클라이언트가 조건부 요청을 했는데 그중 하나가 실패하였음을 의미

- 413 : 요청이 너무 커서 서버가 처리 할 수 없음을 의미

- 414 : 요청 URL이 너무 길어 처리 할 수 없음을 의미

- 415 : 서버가 이해 하지 못하는 유형의 컨텐츠를 요청 하였음을 의미

- 416 : 클라이언트의 요청 내용이 범위가 잘못되었음을 의미

- 417 : 클라이언트 요청 헤더의 Expect에 대해 서버가 만족 하지 않음을 의미

#### 500 - 599 : 서버 에러 상태 코드
올바른 클라이언트 요청에 대해 서버의 문제로 응답 할 수 없음을 의미

- 500 : 서버에 오류가 발생하여 응답 할 수 없음을 의미

- 501 : 클라이언트 요청에 대한 서버의 응답 수행 기능이 없음을 의미(ex] 서버가 지원하지 않는 새로운 Method를 사용하여 요청 - GET2, POST2...)

- 502 : 프락시나 게이트웨이등의 서버에서 응답하며, 서버의 모(엄마)서버에서 오류가 발생하였음을 의미

- 503 : 현재 서버가 유지보수 등의 이유로 일시적인 사용 불가함을 의미

- 504 : 서버에서 다른 서버로 요청을 보냈으나, 응답 지연이 발생하여 처리가 불가함을 의미

- 505 : 서버가 지원할 수 없거나 올바르지 못한 프로토콜로 요청을 받았음을 의미

### 6. CORS란 (Cross Origin Resource Sharing)
> 웹 서버에게 보안 cross-domain 데이터 전송을 활성화하는 cross-domain 접근 제어권을 부여한다.
- 배경
    - 처음 전송되는 리소스의 도메인과 다른 도메인으로부터 리소스가 요청될 경우 해당 리소스는 cross-origin HTTP 요청에 의해 요청된다.
    - 보안 상의 이유로, 브라우저들은 스크립트 내에서 초기화되는 cross-origin HTTP 요청을 제한한다.
        - 예를 들면, XMLHttpRequest는 same-origin 정책을 따르기에 XMLHttpRequest을 사용하는 웹 애플리케이션은 자신과 동일한 도메인으로 HTTP 요청을 보내는 것만 가능했다.
        - 웹 애플리케이션을 개선시키기 위해, 개발자들은 브라우저 벤더사들에게 XMLHttpRequest가 cross-domain 요청을 할 수 있도록 요청했고 이에 따라 CORS가 생겼다.
- 과정
    - CORS 요청 시에는 미리 OPTIONS 주소로 서버가 CORS를 허용하는지 물어본다.
    - 이때 Access-Control-Request-Method로 실제로 보내고자 하는 메서드를 알리고,
    - Access-Control-Request-Headers로 실제로 보내고자 하는 헤더들을 알린다.
    - Allow 항목들은 Request에 대응되는 것으로, 서버가 허용하는 메서드와 헤더를 응답하는데 사용된다. (preflight)
    - Request랑 Allow가 일치하면 CORS 요청이 이루어진다.

> - [https://zamezzz.tistory.com/137](https://zamezzz.tistory.com/137)

### 7. HTTPS / HTTP2
#### 1. HTTPS
> HyperText Transfer Protocol over Secure Socket Layer

- HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, 웹 상에서 정보를 암호화하는 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다.

- HTTPS의 원리 
    - 공개키 알고리즘 (공개키는 암호화에, private 키는 복호화에)
        1. 사용자의 데이터를 공개키로 암호화 (공개키를 얻은 인증된 사용자)
        2. 서버로 전송 (데이터를 가로채도 개인키가 없으므로 복호화할 수 없음)
        3. 서버의 개인키를 통해 복호화하여 요청 처리
    - 사용자 인증 방법  
         
         - 클라이언트가 서버에 접속한 직후에, 서버는 클라이언트에게 이 인증서 정보를 전달한다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증 한 후에 다음 절차를 수행하게 된다 (SSL 인증서에 공개키 포함)
        - 그리고 SSL 인증서 안에는 CA (인증서 발급 조직) 가 있는데, 브라우저가 미리 CA 리스트를 파악하고 있고, 여기에 있는 것이 공인된 CA 다.

#### 2. HTTP2 
> 웹 페이지 로드 시 리소스가 예전에 비해 많이 커지면서 이에대한 부분을 더 효율적으로 만들어주는데 초점을 맞춤 

1.  Binary Framework 

    - HTTP/2 는 TCP 계층과의 사이에 새로운 Binary Framework 를 통해 네트워크 스택을 구성한다.
    - 기존 텍스트 기반 Header 와 Body 대신 HTTP/2 는 전송에 필요한 메시지들을 Binary 단위로 구성하여 더 작은 프레임으로 쪼개서 관리한다.

기존에 텍스트 기반으로 Header 와 Data 가 연결되고 있던 1.1 이하 버전과 다르게 HTTP/2 는 전송에 필요한 메시지들을 Binary 단위로 구성하며 필요 정보를 더 작은 프레임으로 쪼개서 관리한다. 

2. Multiplexing 개선
> 멀티플렉싱이란 한 링크에서 어러개의 신호를 동시에 송수신 할 수 있도록 하는 것

<img src="https://t1.daumcdn.net/cfile/tistory/995998375C17AE5F13"/>

기존 HTTP 1.1 과는 다르게 이것이 가능한 이유는 HTTP/2는 패킷을 Frame 단위로 세분화하여 순서에 상관없이 받는쪽에서 조립하도록 설계하였기 때문이다. 

3.  Header Compression
    - 헤더 압축

4. Server Push  
HTTP/1.1에서 클라이언트는 요청한 HTML문서를 수신한 후  HTML문서를 해석하면서 필요한 리소스를 재 요청하는 반면 HTTP/2에선 Server Push기법을 통해서 클라이언트가 요청하지도 않은 (HTML문서에 포함된 리소스) 리소스를 Push 해주는 방법으로 클라이언트의 요청을 최소화 해서 성능 향상을 이끌어 낸다. 


### 5. HTTP **VS** Socket

## 5. 쿠기와 세션

## 6. 소켓 / 전문 통신

### 1. 소켓
> 두 프로세스간 네트워크를 통해 통신을 수행할 수 있도록 양쪽에 생성되는 링크의 단자  
네트워크 4계층 (Transport)의 TCP/IP와 관련되어 있음 (연결형 서비스)  
- TCP : 패킷 전달 규약 (전달받은 패킷 재조립, 손상 시 재전송 받음 - 흐름제어)
- IP : TCP는 경로에 대한 규약은 아니기 때문에 라우팅 관련 IP와 같이 사용됨 
- 통신을 위해서는 즉, 라우팅을 위한 IP와 프로세스간 구분을 위한 PORT가 필요함

### 2. 전문
> Fixed Length Format  
https://downman.tistory.com/153?category=669234
- Parsing은 안해도 됨 / 바이너리 데이터로 송수신
- fixed length format 이란 전문을 구성하는 field 들의 길이가 입력받을 수 있는 최대사이즈로 고정시키는 방식 
> ex) 이름은 최대 32byte 까지, 주소는 100byte 까지 항목의 길이를 fix 시키는 것  
**자바에서는 Fixed Length 를 구현하는것이 다소 까다로움 C언어 같은 struct 자료구조를 제공하지 않기 때문에, 아래와 같은 방식으로 진행해야 한다.**  
https://cofs.tistory.com/282
1. 수신한 바이너리 데이터를 byte 배열로 할당
2. 미리 정의된 프로토콜에 따라 데이터의 위치를 찾아서 원하는 부분에 활용

### 3. 스프링에서의 소켓통신 구현
#### 클라이언트
~~~java
public void clientSocket(byte[] data) {
    try {
        //IP주소와 포트번호를 입력하여 Socket 통신을 시작합니다.
        Socket sock = new Socket("127.0.0.1", 8200);

        //Socket으로부터 outputStream을 얻습니다.
        OutputStream os = sock.getOutputStream();

        //등록한 OutputStream을 DataOutputStream 방식으로 사용
        DataOutputStream dos = new DataOutputStream(os);

        //byte[] 파일을 object 방식으로 통째로 전송합니다.
        oos.write(data);
    
        // 스트림과 소켓은 잘 닫아준다. 
        oos.close();
        os.close();
        sock.close();
            
        } catch (SocketTimeoutException e){

        // 타임아웃 되었을때
        timeoutErrorHandler(e);

        } catch (IOException e) {

        // IOException 처리는 강제한다.
        e.printStackTrace();
    }

}

~~~

#### 서버
~~~java
public static void serverSocketListener (){
    int port = 8200;
    //Server측에서 사용할 포트번호를 설정한 후 Socket 서버 개방
    ServerSocket sock = new ServerSocket(port);

    //Client로부터 소켓 신호를 기다림
    Socket socket = sock.accept();

    //Socket로부터 InputStream을 등록
    InputStream is = socket.getInputStream();

    //등록한 InputStream을 DataInputStream방식으로 사용
    DataInputStream dis = new DataInputStream(is);
    
    //미리 데이터를 수신할 버퍼 생성 
    byte[] buffer = new byte[1156];

    //미리 지정해 놓은 버퍼에 전송된 byte[] 데이터를 수신
    dis.readFully(buffer);
    
    dis.close();
    is.close();
    socket.close();
    sock.close();
}

~~~


- **DataInputStream / DataOutputStream**
https://docs.oracle.com/javase/6/docs/api/java/io/DataInputStream.html  

1. 외부 라이브러리가 아니라 자바의 스탠다드 라이브러리다

2. DataInputStream 을 사용하면 애플리케이션이 기계에 독립적인 방법으로 기본 입력 스트림에서 원시 Java 데이터 유형을 읽을 수 있다. (8가지 다양한 자료형으로 읽고 쓸 수 있음)

3. 두 클래스는 각각 생성자에서 InputStream과 OutputStream을 받아들임 (API 확인)  
생성자의 arguments 값으로 InputStream, OutputStream을 받아들인다는 의미는 그 하위 클래스들을 받아들인다는 뜻

4. JAVA9 이라면 위 클래스 쓰지 않아도 아래와 같은 코드도 가능  
~~~java
InputStream is;
byte[] array = is.readAllBytes();
~~~
- **java.io의 주요 클래스**  
2-1) InputStream : 
바이트 단위로 데이터를 읽는다  
2-2) OutputStream : 
외부로 데이터를 전송한다

### 3. BufferedInputStream / BufferedOutputStream
> BufferedInputStream / BufferedOutputStream을 사용하면 편리하고 효율적인 입출력을 할 수 있다.

BufferedInputStream을 통하여 원하는 자료를 1바이트 단위로 읽는 read() 메소드를 수행하면 시스템 내부적으로 버퍼를 준비하고 이 버퍼를 이용하여 지정된 파일로부터 버퍼의 크기만큼 한꺼번에 많은 데이터를 가져온다.

이렇게 채워진 버퍼로부터 1 바이트씩 읽어들여 프로그램으로 전달하게 된다.  
이때, 1 바이트씩 읽어들이는 작업은 파일로부터 읽어오는 것이 아닌 준비된 시스템 버퍼에서 읽어오게 되므로, 파일 입력으로 인한 성능 저하를 최소화 할 수 있다.

BufferedOutputStream 역시 마찬가지로 데이터를 1 바이트씩 출력하면 파일이 아닌 준비된 시스템 버퍼에 출력을 쌓는다.  
버퍼가 모두 채워지거나 close(), flush() 명령을 만나면 버퍼의 모든 내용을 하드 디스크 파일에 출력한다.

따라서, BufferedOutputStream 역시 하드 디스크 파일에 대한 출력을 최소화 하여 효율을 향상 시킬 수 있다.

결론적으로 사용자가 BufferedInputStream과 BufferedOutputStream을 이용하여 프로그램을 작성하면 1 바이트씩 읽고 쓰는 모든 작업이 하드 디스크 파일이 아닌 내부적인 버퍼를 대상으로 발생하며, 필요에 따라 버퍼와 하드 디스크 파일간에 입출력이 간헐적으로 발생하므로 전체적인 입출력 성능이 동적으로 향상될 수 있다.



### 4. String 으로 바이트 데이터 만들기
> 보통 은행이나 기타 금융쪽 시스템은 C / C++ 기반인 경우가 많음  
따라서 정해진 buffer로 적절히 끊어서 전송해야 하기 때문에, byte[] 형식으로 변환한 후 socket에 실어서 전송해야 한다. 
~~~java
byte[] res = text.getBytes(StandardCharsets.UTF_8);
~~~
1. **getBytes()** 메소드는 소켓통신 등 특정 상황에서 스트링을 바이트 배열로 만드는 메소드이고, 수신 측에서는 이를 바이트 배열로 읽고 디코딩하면 원본 String을 얻을 수 있다.

### 4.1. Byte - String / String - byte
~~~java
String str1 = “Hello World!”;

// 변수 str1의 바이트 값

// 72101108108111328711111410810033

bytes[] buffers = str1.getBytes(); 

…

// 바이트 배열 자체의 문자열 값

// [B@ca0b6

String buffersArrayString = buffers.toString();

…

// 바이트 배열을 문자열로 변환한 값

// Hello World!

String str2 = new String(buffers);

~~~