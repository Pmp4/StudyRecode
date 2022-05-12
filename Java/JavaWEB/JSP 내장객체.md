# JSP 내장객체
별다른 선언과정과 객체 생성 없이 사용할 수 있는 9개의 객체들을 웹 컨테이너가 제공한다.
- 입출력 관련 내장 객체 (request, response, out)
- 외부 환경 정보 제공 내장 객체 (session, application)
- 서블릿 관련 내장 객체 (pageContext, config)
- 예외 관련 내장 객체 (exception)

주요 객체 : pageContext, request, session, application

### request 내장 객체 (입출력 관련)
웹 브라우저에서 JSP 페이지로 전달되는 정보의 모임(응답 페이지)<br>
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
