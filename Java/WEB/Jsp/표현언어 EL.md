# 🐚 EL 표현언어 🐚
```JSP 페이지```에서 사용되는 ```Java코드```를 대신할 수 있는 <b>액션 태그 엘리먼트</b>. 즉 Java코드를 대신하는 새로운 언어

표현 언어는 Jsp의 표현식 ```<%= %>```을 대신하는 효과가 있다.

```jsp
<%= person.getAddress() %>  //Jsp 표현식
${person.address} //el 표현언어
```
이 두개의 코드는 동일한 결과값을 가진다.

## 표현 언어의 개요
과거의 표현언어는 JSTL에 상당히 종속적인 면을 가지고 있었지만, JSP 2.0 스펙에 표함되며 JSP 컨테이너 자신이 표현 언어(EL)의 표현식을 해석할 수 있게 되었고, 표준과 커스텀 액션 태그 HTML 같은 템플릿 텍스트와 같이 자바 코드를 사용해야 했던 모든 곳에서 표현 언어(EL)을 사용할 수 있게 되었다.

표현언어(EL)은 null 값을 가지는 변수에 관대하고, 데이터 형변환에 간접적으로 접근하며 자동적으로 해준다.
> 값이 없을 경우(null)에 대해 자동으로 처리해주고, 형변환을 자동으로 해준다.<br/>
> Java EE8 : jsp2.3, servlet 4.0, EL 3.0, JSTL 1.2

### 표현언어 표현식의 기능
- 변수와 연산자를 포함한다.
- Jsp의 영역(page, request, session, application)에 저장된 어떤 속성 및 자바빈 객체라도 표현언어의 변수로서 사용가능
- 내장 객체를 지원한다(가지고있다).

### 표현언어의 작성 방법
- 표현언어 표현식에는 숫자, 문자, boolean 값, null 같은 상수값(리터럴)들을 포함할 수 있다.
- 표현언어는 $와 표현식, {}를 사용해서 표현한다. ```${pageContext.request.contextPath)```
<br/>

## 표현언어의 내장 객체
|내장 객체|설명|
|--|--|
|pageScope|모든 page 영역 객체들에 대한 컬렉션|
|requestScope|모든 request 영역 객체들에 대한 컬렉션|
|sessionScope|모든 session 영역 객체들에 대한 컬렉션|
|applicationScope|모든 application 영역 객체들에 대한 컬렉션|
|param|모든 request 파라미터들을 문자열로 가진 컬렉션|
|paramValues|모든 request 파라미터들을 파라미터당 문자열 배열로 가 진 컬렉션|
|header|HTTP 요청 헤더를 문자열로 가진 컬렉션|
|headerValues|HTTP 요청 헤더들을 헤더당 문자열 배열로 가진 컬렉션|
|cookie|모든 쿠키의 컬렉션|
|initParam|모든 어플리케이션의 초기화 파라미터의 이름 컬렉션|
|pageContext|현재 페이지를 위한 javax.servlet.jsp.PageContext|
<br/>

## 예제
### el표현식
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>elTest.jsp</title>
</head>
<body>
<!-- 
	표현언어-el : 기존의 Jsp 표현식을 대체하는 효과
	el 표현식에는 연산자 사용 가능, 내장객체 지원함
	null에 관대하고 형변환도 자동으로 해줌
 -->
 
	<h1>el 표현식</h1>
	<p>\${2+5} : ${2+5 }</p>
	<p>\${10/3} : ${10/3}</p>
	<p>\${10%3} : ${10%3}</p>
	<p>\${header.host} : ${header.host}</p>
	<p>\${header["user-agent"]} : ${header["user-agent"]}</p>
	
	<h2>기존방식</h2>
	<%
		 String host = request.getHeader("host");
		 String agent = request.getHeader("user-agent");
	%>
	<p>host : <%=host %></p>
	<p>user-agent : <%=agent %></p>
</body>
</html>
```

### 파라미터 받아오기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<H3>표현언어 -파라미터값 처리</H3>
	<%
		/* request.setCharacterEncoding("UTF-8"); */
	%>
	<fmt:requestEncoding value="UTF-8"/>
	<P>
	<FORM action="paramTest.jsp" method="post">
		이름 : <input type="text" name="name" value="${param['name']}">
		<input type="submit" value="확인">
	</FORM>
	
	<%
		//기본 방식
		String name = request.getParameter("name");
	%>
	<h2>기존 방식 - 파라미터 값</h2>
	<p>name : <%=name %></p>
	<h2>el 표현식 이용 - param</h2>
	<p>name : ${param.name }</p>
</body>
</html>
```
