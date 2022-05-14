# JSP 내장객체
별다른 선언과정과 객체 생성 없이 사용할 수 있는 9개의 객체들을 웹 컨테이너가 제공한다.
- 입출력 관련 내장 객체 (request, response, out)
- 외부 환경 정보 제공 내장 객체 (session, application)
- 서블릿 관련 내장 객체 (pageContext, config)
- 예외 관련 내장 객체 (exception)

주요 객체 : request, session, application, pageContext
|메서드|리턴 타입|설명|
|--|--|--|
|setAttribute(String key, Object value)|void|주어진 key 속성의 값을 value로 지정한다.|
|getAttributeNames()|java.util.Enumeration|모든 속성의 이름을 구한다|
|getAttribute(String key)|Object|주어진 key 속성의 값을 얻어낸다|
|removeAttribute(String key)|void|주어진 key 속성의 값을 제거한다.|

속성(attribute)값을 저장하고 읽을 수 있는 메서드인 setAttribute(), getAttribute() 메서드를 제공함

<br>


### request 내장 객체 (입출력 관련)
웹 브라우저에서 ```JSP 페이지로 전달되는 정보```의 모임(응답 페이지)<br>
클라이언트가 전송한 요청 정보를 제공하는 것이 request 기본 객체이다.
|메서드|설명|
|--|--|
|String getParameter(name)|이름이 name인 파라미터에 할당된 값을 리턴하며, 지정된 파라미터 값이 없으면 null값을 리턴함
|String[] getParameterValues(name) |이름이 name인 파라미터의 모든 값을 String 배열로 리턴함 Checkbox 에서 주로 사용됨|
|Enumeration getParameterNames()|요청에 사용된 모든 파라미터 이름을 java.util.Enumeration 타입으로 리턴함|

```jsp
//요청
<%@ page contentType = "text/html; charset=utf-8" %>
<html>
  <head><title>폼 생성</title></head>
  <body>
    폼에 데이터를 입력한 후 '전송' 버튼을 클릭하세요.
    <form action="/test/viewParameter.jsp" method="post">
      이름: <input type="text" name="name" size="10"> <br>
      주소: <input type="text" name="address" size="30"> <br>
      좋아하는 동물:
        <input type="checkbox" name="pet" value="dog">강아지
        <input type="checkbox" name="pet" value="cat">고양이
        <input type="checkbox" name="pet" value="pig">돼지
      <br>
      <input type="submit" value="전송">
    </form>
  </body>
</html>
```jsp

```jsp
//응답
<%@ page contentType = "text/html; charset=utf-8" %>
<%
  request.setCharacterEncoding("utf-8");
  
  String name = request.getParameter("name");
  String address = request.getParameter("address");
  String[] values = request.getParameterValues("pet");
%>
<html>
  <head></head>
  <body>
  </body>
</html>
```

request 기본객체가 제공하는 기능
- 클라이언트와 관련된 정보 읽기 기능
- 서버와 관련된 정보 읽기 기능
- 클라이언트가 전송한 요청 파라미터 읽기 기능
- 클라이언트가 전송한 요청 헤더 읽기 기능
- 클라이언트가 전송한 쿠키 읽기 기능
- 속성 처리 기능

#### 클라이언트 정보 및 서버 정보 읽기
|메서드|설명|
|--|--|
|String getRequestURI()|요청에 사용된 URL 로부터 URI를 리턴|
|String getRemoteHost()|클라이언트의 호스트 이름을 리턴|
|String getRemoteAddr()|클라이언트의 IP 주소를 리턴|
|String getContextPath()|해당 JSP 페이지가 속한 웹 어플리케이션의 컨텍스트 경로를 리턴|

### pageContext 내장 객체 (서블릿 관련)
```현재 JSP 페이지```의 컨텍스트를 나타내며, 주로 다른 내장 객체를 얻어내거나, 페이지의 흐름제어, 에러 데이터를 얻어낼 때 사용된다.

### session 내장 객체
웹 브라우저의 요청시 요청한 웹 브라우저(세션)에 관한 정보를 저장하고 관리하는 내장 객체<br>
웹 브라우저(클라이언트)당 ```1개```가 할당된다.

### application 내장 객체
웹 어플리케이션의 설정 정보를 갖는 context와 관련이 있는 객체<br>
application 내장 객체는 웹 어플리케이션당 1개의 객체가 생성됨

|메서드|설명|
|--|--|
|String getRealPath(path)|지정한 경로를 웹 어플리케이션 시스템 상의 경로로 변경하여 리턴함|

### config 내장 객체
서블릿이 초기화될 때 참조해야 하는 정보를 가지고 있다가 전달해준다<br>
컨테이너당 1개의 객체가 생성된다.
|메서드|설명|
|--|--|
|Enumeration getInitParameterName()|모든 초기 파라미터 이름을 리턴|
|String getInitParameter(name)|이름이 name인 초기 파라미터의 값을 리턴|
|String getServletName()|서블릿의 이름을 리턴|
|ServletContext getServletContext()|실행하는 서블릿 ServletContext 객체를 리턴|


