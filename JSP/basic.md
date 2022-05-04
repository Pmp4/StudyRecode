# JSP

### response 내장 객체
웹 브라우저로 응답할 응답 정보를 가지고 있음

서블릿에서 <b>HttpServletResponse</b>를 뜻하고 Jsp에선 response객체로 사용된다.

|메서드|설명|
|--|--|
|void <b>setHeader(name, value)</b>|name에 해당하는 속성을 value값으로 설정|
|void <b>setContentType(type)</b>|dd|
|void <b>sendRedirect(url)</b>|지정된 url로 페이지가 이동한다.|

## HTTP 프로토콜의 이해
프로토콜(Protocol) - 통신규약, 규칙
- 통신 시스템이 데이터를 교환하기 위해 사용하는 통신규칙
- 컴퓨터간에 정보를 주고받을 때의 통신방법에 대한 규칙과 약속

### HTTP 프로토콜
- 웹 브라우저와 웹 서버 사이의 데이터 통신 규칙
- 웹 브라우저는 HTTP 요청 형식에 따라 웹 서버에 데이터를 보냄
- 보내는 데이터는 HTTP 응답 형식에 맞추어 보냄

#### 특징
stateless, connectless의 특징을 갖는 프로토콜

요청(request)/응답(response)를 끊임없이 주고 받는다.
- 클라이언트는 요청, 서버는 응답(HTML문서)
