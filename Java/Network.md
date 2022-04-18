# 🛰 Networking 🛰
### 네트워크란
1. <b>두 대 이상</b>의 컴퓨터를 연결하여 데이터 및 자원을 공유하는 것이 네트워크의 정의다.
2. 네트워크에 연결된 모든장치를 <b>'노드(Node)'</b>라고 한다

Java에서 네트워크 데이터 통신을 위해 <b>'java.net'</b> 패키지를 지원한다.

### 통신의 3대 요소
- 서버(Server) : 서비스 제공(파일 서버, 메일 서버, 어플리케이션 서버), 서버 프로그램이 필요
- 클라이언트(Client) : 서비스 사용자, 클라이언트 프로그램이 필요
- 네트워크(Network) : '서버 - 클리이언트' 연결
> 서버 프로그램 : 서버가 서비스를 제공하기 위해서 필요<br>
> 클라이언트 프로그램 : 서비스를 제공받기 위해서 서버프로그램과 연결하기 위해 필요

### 클라이언트/서버 통신 과정
- 웹브라우저 프로그램 -> 포트 : 8080 -> 웹 서버
- FTP 프로그램 -> 포트 : 21 -> FTP 서버
- DB 프로그램 -> 포트 : 1521 -> DBMS

클라이언트와 서버간에 통신을 위해선 공통적으로 <b>'포트번호'</b>가 필요하다.


## 🚧 IP주소(IP Address)
호스트를 구별하는 고유한 값, 4byte(32bit)의 정수로 구성되어 4개의 정수가 마침표를 구분자로 a.b.c.d의 형식으로 표현

IP주소 a.b.c.d에서 a.b.c는 네트워크 주소, d는 호스트주소로 나눌 수 있다.
> 예 : 192.168.10.100 => 네트워크 주소 '192.168.10', 호스트 주소 '100'

### InetAddress
자바에서 <b>IP주소</b>를 다루기 위한 클래스로 InetAddress를 제공한다.
|메소드|설명|
|------|---|
|byte[] <b>getAddress()</b>|테스트2|
|static InetAddress[] <b>getAllByName(String host)</b>|host에 지정된 모든host의 IP주소를 배열에 담아 리턴한다|
|static InetAddress <b>getByAddress(byte[] host)</b>|Byte배열을 통해 IP주소를 얻는다|
|static InetAddress <b>getByName(String host)</b>|도메인명을 통해 IP주소를 얻는다|
|String <b>getHostAddress()</b>|호스트의 IP주소를 리턴한다|
|String <b>getHostName()</b>|테스트2|
|static InetAddress <b>getLocalHost()</b>|지역호스트(본인 컴퓨터의 호스트)의 IP주소를 리턴|
|boolean <b>isMulticastAddress()</b>|IP주소가 멀티캐스트 주소인지 알려준다|
|boolean <b>isLoopbackAddress()</b>|IP주소가 loopback주소(127.0.0.1)인지 알려준다|
```java
public class InetAddressTest {
  public static void main(String[] args) {
    InetAddress ip = null;  //InetAddress는 IP주소 관련 정보를 담고있다.
    InetAddress[] ipArr = null;
    
    try {
      ip = InetAddress.getByName("www.naver.com") //도메인명(host)을 통해 IP주소를 가진 'InetAddress'객체 리턴
      ip.getHostName();     //ip객체의 호스트이름 리턴
      ip.getHostAddress();  //ip객체의 호스트주소 리턴
      
      ipArr = InetAddress.getAllByName("www.naver.com");  //도메인명(host)에 지정된 모든 IP주소를 리턴
    }
  }
}
```

## 소켓 프로그래밍
소켓을 이용한 통신 프로그래밍
> 소켓(socket) - 프로세스간의 통신에 사용되는 양쪽 끝단

소켓 프로그래밍에는 TCP통신, UDP통신으로 두가지 통신방식이 있다.

### TCP 통신 방식
- 양방향 연결 기반 통신
- 신뢰성 있는 데이터 전송(수신여부 확인)
- 연결 후 통신 - 전화 통신
- 데이터 손실 시 재전송
서버와 클라이언트가 TCP 방식으로 통신하기 위해선 서버와 클라이언트의 연결이 이뤄진 후 데이터를 주고 받는다.

#### 통신 순서
```java
ServerSocket serverSocket = new ServerSocket(특정포트); //서버소켓에 특정포트 연결
Socket socket = serverSocket.accept();  //연결요청 여부 확인 후 소켓 생성
```
> 서버 프로그램에서 서버소켓을 사용하여 서버 컴퓨터의 특정 포트에서 클라이언트의 연결 요청을 처리할 준비를 한다.


```java
Socket socket = new Socket("서버의 IP주소", 특정포트);
```
> 클라이언트 프로그램에서 접속할 서버의 IP주소와 포트정보를 가지고 소켓을 생성하여 서버에 연결을 요청한다.


서버소켓은 연결요청을 받으면 서버에 새로운 소켓을 생성하여 클라이언트 소켓과 연결되도록 한다.


### UDP 통신방식
- 비연결 기반 통신 방식
- 신뢰성 없는 데이터 전송(수신여부 확인X)
- 연결 없이 통신 - 우편물 배달방식
- 데이터 손실 가능성
- 전송속도가 빠르다

