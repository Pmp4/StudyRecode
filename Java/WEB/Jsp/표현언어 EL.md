# π EL ννμΈμ΄ π
```JSP νμ΄μ§```μμ μ¬μ©λλ ```Javaμ½λ```λ₯Ό λμ ν  μ μλ <b>μ‘μ νκ·Έ μλ¦¬λ¨ΌνΈ</b>. μ¦ Javaμ½λλ₯Ό λμ νλ μλ‘μ΄ μΈμ΄

νν μΈμ΄λ Jspμ ννμ ```<%= %>```μ λμ νλ ν¨κ³Όκ° μλ€.

```jsp
<%= person.getAddress() %>  //Jsp ννμ
${person.address} //el ννμΈμ΄
```
μ΄ λκ°μ μ½λλ λμΌν κ²°κ³Όκ°μ κ°μ§λ€.

## νν μΈμ΄μ κ°μ
κ³Όκ±°μ ννμΈμ΄λ JSTLμ μλΉν μ’μμ μΈ λ©΄μ κ°μ§κ³  μμμ§λ§, JSP 2.0 μ€νμ νν¨λλ©° JSP μ»¨νμ΄λ μμ μ΄ νν μΈμ΄(EL)μ ννμμ ν΄μν  μ μκ² λμκ³ , νμ€κ³Ό μ»€μ€ν μ‘μ νκ·Έ HTML κ°μ ννλ¦Ώ νμ€νΈμ κ°μ΄ μλ° μ½λλ₯Ό μ¬μ©ν΄μΌ νλ λͺ¨λ  κ³³μμ νν μΈμ΄(EL)μ μ¬μ©ν  μ μκ² λμλ€.

ννμΈμ΄(EL)μ null κ°μ κ°μ§λ λ³μμ κ΄λνκ³ , λ°μ΄ν° νλ³νμ κ°μ μ μΌλ‘ μ κ·Όνλ©° μλμ μΌλ‘ ν΄μ€λ€.
> κ°μ΄ μμ κ²½μ°(null)μ λν΄ μλμΌλ‘ μ²λ¦¬ν΄μ£Όκ³ , νλ³νμ μλμΌλ‘ ν΄μ€λ€.<br/>
> Java EE8 : jsp2.3, servlet 4.0, EL 3.0, JSTL 1.2

### ννμΈμ΄ ννμμ κΈ°λ₯
- λ³μμ μ°μ°μλ₯Ό ν¬ν¨νλ€.
- Jspμ μμ­(page, request, session, application)μ μ μ₯λ μ΄λ€ μμ± λ° μλ°λΉ κ°μ²΄λΌλ ννμΈμ΄μ λ³μλ‘μ μ¬μ©κ°λ₯
- λ΄μ₯ κ°μ²΄λ₯Ό μ§μνλ€(κ°μ§κ³ μλ€).

### ννμΈμ΄μ μμ± λ°©λ²
- ννμΈμ΄ ννμμλ μ«μ, λ¬Έμ, boolean κ°, null κ°μ μμκ°(λ¦¬ν°λ΄)λ€μ ν¬ν¨ν  μ μλ€.
- ννμΈμ΄λ $μ ννμ, {}λ₯Ό μ¬μ©ν΄μ νννλ€. ```${pageContext.request.contextPath)```
<br/>

## ννμΈμ΄μ λ΄μ₯ κ°μ²΄
|λ΄μ₯ κ°μ²΄|μ€λͺ|
|--|--|
|pageScope|λͺ¨λ  page μμ­ κ°μ²΄λ€μ λν μ»¬λ μ|
|requestScope|λͺ¨λ  request μμ­ κ°μ²΄λ€μ λν μ»¬λ μ|
|sessionScope|λͺ¨λ  session μμ­ κ°μ²΄λ€μ λν μ»¬λ μ|
|applicationScope|λͺ¨λ  application μμ­ κ°μ²΄λ€μ λν μ»¬λ μ|
|param|λͺ¨λ  request νλΌλ―Έν°λ€μ λ¬Έμμ΄λ‘ κ°μ§ μ»¬λ μ|
|paramValues|λͺ¨λ  request νλΌλ―Έν°λ€μ νλΌλ―Έν°λΉ λ¬Έμμ΄ λ°°μ΄λ‘ κ° μ§ μ»¬λ μ|
|header|HTTP μμ²­ ν€λλ₯Ό λ¬Έμμ΄λ‘ κ°μ§ μ»¬λ μ|
|headerValues|HTTP μμ²­ ν€λλ€μ ν€λλΉ λ¬Έμμ΄ λ°°μ΄λ‘ κ°μ§ μ»¬λ μ|
|cookie|λͺ¨λ  μΏ ν€μ μ»¬λ μ|
|initParam|λͺ¨λ  μ΄νλ¦¬μΌμ΄μμ μ΄κΈ°ν νλΌλ―Έν°μ μ΄λ¦ μ»¬λ μ|
|pageContext|νμ¬ νμ΄μ§λ₯Ό μν javax.servlet.jsp.PageContext|
<br/>

## μμ 
### elννμ
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
	ννμΈμ΄-el : κΈ°μ‘΄μ Jsp ννμμ λμ²΄νλ ν¨κ³Ό
	el ννμμλ μ°μ°μ μ¬μ© κ°λ₯, λ΄μ₯κ°μ²΄ μ§μν¨
	nullμ κ΄λνκ³  νλ³νλ μλμΌλ‘ ν΄μ€
 -->
 
	<h1>el ννμ</h1>
	<p>\${2+5} : ${2+5 }</p>
	<p>\${10/3} : ${10/3}</p>
	<p>\${10%3} : ${10%3}</p>
	<p>\${header.host} : ${header.host}</p>
	<p>\${header["user-agent"]} : ${header["user-agent"]}</p>
	
	<h2>κΈ°μ‘΄λ°©μ</h2>
	<%
		 String host = request.getHeader("host");
		 String agent = request.getHeader("user-agent");
	%>
	<p>host : <%=host %></p>
	<p>user-agent : <%=agent %></p>
</body>
</html>
```

### νλΌλ―Έν° λ°μμ€κΈ°
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
	<H3>ννμΈμ΄ -νλΌλ―Έν°κ° μ²λ¦¬</H3>
	<%
		/* request.setCharacterEncoding("UTF-8"); */
	%>
	<fmt:requestEncoding value="UTF-8"/>
	<P>
	<FORM action="paramTest.jsp" method="post">
		μ΄λ¦ : <input type="text" name="name" value="${param['name']}">
		<input type="submit" value="νμΈ">
	</FORM>
	
	<%
		//κΈ°λ³Έ λ°©μ
		String name = request.getParameter("name");
	%>
	<h2>κΈ°μ‘΄ λ°©μ - νλΌλ―Έν° κ°</h2>
	<p>name : <%=name %></p>
	<h2>el ννμ μ΄μ© - param</h2>
	<p>name : ${param.name }</p>
</body>
</html>
```
