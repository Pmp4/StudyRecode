# 🛰 Java Networking 🛰
### 네트워크란
1. <b>두 대 이상</b>의 컴퓨터를 연결하여 데이터 및 자원을 공유하는 것이 네트워크.
2. 네트워크에 연결된 모든장치를 <b>'노드(Node)'</b>라고 한다.
3. 노드가 다른노드에게 하나 이상의 서비스를 하는 것을 <b>호스트</b>라 부른다.

Java에서 네트워크 데이터 통신을 위해 <b>'java.net'</b> 패키지를 지원한다.
> Java는 네트워크를 염두해두고 만들어진 최초의 프로그래밍언어이다.

#### 네트워크 기본개념
|계층|설명|
|---|---|
|Application 계층 (실제 구현되는 프로그램)|도착한 데이터를 사용자에게 전달하는 계층|
|전송 계층 (TCP/UDP)|데이터의 도착여부를 체크하는 계층|
|인터넷 계층|패킷이 통과하는 계층|
|물리 계층 (이더넷)|기본적 아날로그 신호, 보낼 땐 디지털 신호로, 받을 땐 아날로그 신호로 변환|

#### #주요용어
- Protocol : 네트워크 통신의 전송 규약, 즉 약속
- IP : 컴퓨터의 고유 번호. 해당 주소를 통해 서로 통신이 가능하게 한다.
- TCP : 패킷을 받았다는 확인을 전송하고, 잃어버린 패킷에 대해서는 재전송을 요구한다. 따라서 상당한 양의 오버헤드를 동반. (HTTP, FTP 등)
- UDP : 데이터가 목적지에 정확히 도착했는지, 또는 보내진 순서대로 도착했는지를 전혀 보장해주지 않는 신뢰성없는 프로토콜 (SMTP 등)
- Port : 컴퓨터 내의 서버 프로그램들을 구별하기 위한 번호. 물리적인 포트(직렬포트, 병렬포트 등)를 말하는것은 아니다.
- Socket : 서버와 클라이언트의 연결점
- LoopBack : 패킷을 던졌을 때 자기 자신에게 돌아오는 주소. (자신의 주소를 말하며, 외부에 공개된 나의 주소와는 구별되어있는 공통된 주소)

### 통신의 3대 요소
- 서버(Server) : 서비스 제공(파일 서버, 메일 서버, 어플리케이션 서버), 서버 프로그램이 필요
- 클라이언트(Client) : 서비스 사용자, 클라이언트 프로그램이 필요
- 네트워크(Network) : '서버 - 클리이언트' 연결
> 서버 프로그램 : 서버가 서비스를 제공하기 위해서 필요<br>
> 클라이언트 프로그램 : 서비스를 제공받기 위해서, 서버프로그램과 연결하기 위해 필요

### 클라이언트/서버 통신 과정
- 웹브라우저 프로그램 -> 포트 : 8080 -> 웹 서버
- FTP 프로그램 -> 포트 : 21 -> FTP 서버
- DB 프로그램 -> 포트 : 1521 -> DBMS

클라이언트와 서버간에 통신을 위해선 공통적으로 <b>'[포트번호](#주요용어)'</b>가 필요하다.


## 🚧 IP주소(IP Address)
호스트를 구별하는 고유한 값, 4byte(32bit)의 정수로 구성되어 4개의 정수가 마침표를 구분자로 a.b.c.d의 형식으로 표현

IP주소 a.b.c.d에서 a.b.c는 네트워크 주소, d는 호스트주소로 나눌 수 있다.
> 예 : 192.168.10.100 => 네트워크 주소 '192.168.10', 호스트 주소 '100'

### InetAddress
자바에서 <b>IP주소</b>를 다루기 위한 클래스로 InetAddress를 제공한다.
IP와 관련된 정보들을 저장할 수 있다.
|메소드|설명|
|------|---|
|byte[] <b>getAddress()</b>|IP주소를 byte배열로 리턴|
|static InetAddress[] <b>getAllByName(String host)</b>|host에 지정된 모든host의 IP주소를 배열에 담아 리턴한다|
|static InetAddress <b>getByAddress(byte[] host)</b>|Byte배열을 통해 IP주소를 얻는다|
|static InetAddress <b>getByName(String host)</b>|도메인명을 통해 IP주소를 얻는다|
|String <b>getHostAddress()</b>|호스트의 IP주소를 리턴한다|
|String <b>getHostName()</b>|호스트의 이름을 리턴|
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

## 🏚 URL
서버가 제공하는 자원에 접근할 수 있는 주소

<b>형태</b> => [프로토콜](#주요용어)://호스트명:[포트번호](#주요용어)/경로명/파일명?쿼리스트링#참조
> 호스트명 : 자원을 제공하는 서버의 <b>이름</b>(www.naver.com)<br>
> 경로명 : 접근하려는 자원이 저장된 서버상의 위치, 디렉토리(/book/source/)<br>
> 파일명 : 접근하려는 자원의 이름(a.jsp), HTML에서 데이터를 전송하는 위치(form태그의 action속성)<br>
> 쿼리스트링 : URL에서 ?이후의 부분(id=hong), HTML에서 전송하는 데이터의 종류(form태그의 method속성이 get일 경우)<br>
> 참조 : URL에서 #이후의 부분

자바에서 URL을 다루기 위한 클래스로 URL클래스를 제공한다.
|메서드|설명|
|||

## 🏛 소켓 프로그래밍
소켓을 이용한 통신 프로그래밍
> 소켓(socket) - 프로세스간의 통신에 사용되는 양쪽 끝단, 서버와 클라이언트의 연결점

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

