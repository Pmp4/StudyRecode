# MVC (Model View Controller)
기존 Model1 방식 :  JSP 문서가 Model, View, Controller 역할을 함께 수행
- 장점 : 단순한 페이지 흐름에는 간편하고 빠르게 개발 가능
- 단점 : 복잡해질수록 유지보수가 어렵다.


Model2



### Controller
```java
public class TravelController extends HttpServlet {
  private static final long serialVersionUID = 1L;

  protected void doGet(HttpServletRequest request, HttpServletResponse response) 
      throws ServletException, IOException {
    requestProcess(request, response);
	}

  protected void doPost(HttpServletRequest request, HttpServletResponse response) 
      throws ServletException, IOException {
    requestProcess(request, response);
  }

  //doGet, doPost의 요청을 한번에 처리할 method
  private void requestProcess(HttpServletRequest request, HttpServletResponse response) 
      throws ServletException, IOException{

    request.setCharacterEncoding("UTF-8");
    response.setCharacterEncoding("UTF-8");
		
    //1. 파타미터 받아오기
    String [파라미터 받아올 변수명] = request.getParameter("[파라미터]");
		
    //2. 비즈니스 로직처리(DB Model)
    //Model 클래스는 'POJO:Plain Old Java Object'
    [Model클래스 이름] model = new [~~~~~~]();
    String result = model.[model method]([파라미터 값]);
		
    //3. Jsp문서로 forwarding (기본 model1에선 결과처리부분)
    request.setAttribute("result", result); 
    //request에 attribute (forward된 문서는 같은 request를 공유한다.)
		
    RequestDispatcher dispatcher = request.getRequestDispatcher("/tips/travelResult.jsp");
    //View로 넘기기 위해 Jsp문서로 forward
    
    dispatcher.forward(request, response);
	}
}
```

### Model
```java

public class TravelExpert {
  public String [DB작업할 메서드](String [~~~]) {
    ~~~~
    ~~~~
    //model부분은 기존 DAO, Service랑 같은 곳이라고 생각하면 된다.
  }
}
```

### View
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
	String result = (String)request.getAttribute("[~~~]"); 
  //forward는 같은 request를 공유하기 때문 속성 값을 받아올 수 있다.
%>
	<h1>view 페이지</h1>
	<p><%=result %></p>
</body>
</html>
```
