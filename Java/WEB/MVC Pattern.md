# MVC (Model View Controller)
기존 Model1 방식 :  JSP 문서가 Model, View, Controller 역할을 함께 수행
- 장점 : 단순한 페이지 흐름에는 간편하고 빠르게 개발 가능
- 단점 : 복잡해질수록 유지보수가 어렵다.




## MVC 사용방법 1

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
```jsp
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

위와 같은 방식으로 사용시 여러 요청에 맞게 Controller를 하나하나 만들어줘야하는 문제점이 있다.
<br>

## MVC 사용방법 2(하나의 컨트롤러에서 모든 요청을 처리)
사용자가 어떠한 요청을 했는지 판단하기 위해서 ```Command Pattern```을 사용하게 된다.(보편적 사용)<br>
```Command Pattern```의 방식으로는 두가지가 있는데, 요청 파라미터로 명령어 전달 또는 요청 URI 자체를 명령어로 사용하는 방법이 있다.<br>
요청 파라미터를 통한 명령어 전달은 정보가 웹 브라우저를 통해 노출되어 보안성에 취약점이 있어 요청 URI 자체를 명령어로 사용할 것이다.

> 예시 : http://localhost:9090/mymvc/tips/book.do<br>
> ```/mymvc/tips/book.do``` 이 부분이 하나의 명령어가 된다.
<br>

### 1. ```CommandPro.properties``` 파일을 만들어 Controller를 매핑 (명령어와 명령어 처리 클래스)
```
/tips/book.do=com.tips.controller.BookController
/tips/travel.do=com.tips.controller.TravelController
```



- web.xml에서 URL과 Servlet 매핑
- 













