[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 소켓 / 전문 통신
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