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
    
### 4. HTTP Keep-Alive

### 5. HTTP Status Code

### 6. CORS

### 7. HTTPS

### 8. HTTP2

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