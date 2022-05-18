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
좌측 URI 명령어를 받으면 우측 클래스로 처리하겠다는 정보를 가진 파일을 만들어둔다. (Java의 ```Properties(HashMap)``` 클래스 사용)

### 2. 명령어를 처리하는 슈퍼 인터페이스 작성
각각의 명령어 처리 클래스를 하나의 클래스로 묶어줄 ```Controller.java``` 파일을 만들고 각각의 명령어 처리 클래스에 구현할 수 있도록 한다.(다형성을 사용해 처리 로직을 보다 간편하게 만들기위해)
```java
package com.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Controller {
	public String requestProcess(HttpServletRequest request,
			HttpServletResponse response) throws Throwable;
	//forward일 경우
	
	public boolean isRedirect();	
	//request.sendRedirect()일 경우
}
```

### 3. 슈퍼 인터페이스를 구현하는 명령어 처리 클래스 생성
각각의 요청에 맞는 Controller 클래스 파일 슈퍼 인터페이스를 구현하도록 생성한다.
<br>
예시)
```java
package com.tips.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.controller.Controller;
import com.tips.model.BookExpert;

public class BookController2 implements Controller{

	@Override
	public String requestProcess(HttpServletRequest request, HttpServletResponse response) throws Throwable {
		//1. 파라미터 값 얻어오기
		String lang=request.getParameter("lang");

		//2. 비즈니스 로직 처리(DB Model)
		BookExpert model = new BookExpert();
		String result=model.getTip(lang);

		//3. 속성 값을 저장 후 forward 할 주소 리턴
		request.setAttribute("result", result);

		return "/tips/bookResult.jsp";
	}

	@Override
	public boolean isRedirect() {
		return false; //forward
	}
}
```


### 4. web.xml에서 URL과 Servlet 매핑 (모든 URL요청이 하나의 Servlet으로 처리할 수 있도록)
```xml
<servlet>
	<servlet-name>dispatcherServlet</servlet-name>
	<servlet-class>com.controller.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>configFile</param-name>
  		<param-value>/config/CommandPro.properties</param-value>
  	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>dispatcherServlet</servlet-name>
  	<url-pattern>*.do</url-pattern>
</servlet-mapping>
```

### 5. 모든을 처리할 Controller 클래스 ```DispatcherServlet``` 생성
1. init()에서 매핑 파일을 읽어서 Properties 컬렉션에 담아 놓는다.
	- init()은 Servlet 생성 후 초기화할 때 단 한번만 호출되는 메서드
2. 매핑파일이 들어있는 ```Properties``` 컬렉션에서 ```사용자의 URI(명령어)```에 해당하는 ```명령어 처리 클래스(직원) Controller(BookController)```를 찾아 해당 Controller에게 작업 수행하게 한다.
3. 결과를 return() 받아서(requestProcess()) 해당 View 페이지로 forward(또는 sendRedirect)한다. 
```java
package com.controller;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.Properties;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//@WebServlet("/DispatcherServlet")
public class DispatcherServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
    
	private Properties props;
	
	//해당 서블릿이 요청될때 최초로 한번만 호출되는 메서드
	@Override
	public void init(ServletConfig config) throws ServletException {
		//매핑 파일을 읽어서 Properties 컬렉션에 담아 놓는다
		//web.xml에서 <init-param>의 값 읽기-CommandPro.properties 파일
		String configFile = config.getInitParameter("configFile"); //=>/config/CommandPro.properties
		System.out.println("configFile="+configFile);
		
		//매핑 파일의 실제 경로 구하기
		String realPath
		=config.getServletContext().getRealPath(configFile);
		System.out.println("realPath="+realPath+"\n");
		
		props=new Properties();
		FileInputStream fis=null;
		try {
			fis=new FileInputStream(realPath);
			props.load(fis);
			//=> CommandPro.properties파일의 정보를 Properties객체에 저장
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(fis!=null) fis.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		requestPro(request, response);
	}

	protected void doPost(HttpServletRequest request, 
			HttpServletResponse response) 
					throws ServletException, IOException {
		requestPro(request, response);	
	}

	private void requestPro(HttpServletRequest request, 
			HttpServletResponse response) 
					throws ServletException, IOException{
		//매핑파일을 참고해서 직원을 구해서 일시킨다
		
		//매핑파일이 들어있는 Properties 컬렉션에서 사용자의 URI(/tips/book.do)에 해당하는
		//직원 Controller(BookController)를 구해서, 직원 Controller에게 일시킨다
		//(직원 Controller의 메서드 호출) 
		//=> 그리고 나서, 결과를 리턴 받아서 해당 뷰 페이지로 포워딩시킨다
		
		request.setCharacterEncoding("utf-8");
		response.setCharacterEncoding("utf-8");
		
		//1. 요청 URI 에서 명령어 추출
		//URI 읽어오기
		String uri=request.getRequestURI(); //=> /mymvc/tips/travel.do
		System.out.println("uri="+uri);
		
		//명령어 => contextpath 제거 : /tips/travel.do
		String ctxPath=request.getContextPath(); //=> /mymvc		
		if(uri.indexOf(ctxPath)==0) {
			uri=uri.substring(ctxPath.length()); //=> /tips/travel.do
		}
		
		System.out.println("ctxPath="+ctxPath+", 명령어:"+ uri);
		
		//매핑파일
		//=>/tips/travel.do=com.tips.controller.TravelController2
		//key: /tips/travel.do,  value : TravelController2
		String val = props.getProperty(uri);
		System.out.println("명령어 처리 클래스:"+ val);
		
		try {
			Class commandClass= Class.forName(val); //String=>Class
			//=> 해당 문자열을 클래스로 만든다.
			
			Controller controller = (Controller) commandClass.newInstance();
			//=> 해당클래스의 객체를 생성
			
			//명령어 처리 클래스의 메서드 호출
			String viewPage=controller.requestProcess(request, response);
			System.out.println("viewPage="+viewPage);
			
			if(controller.isRedirect()) {  //redirect
				System.out.println("redirct!\n");
				response.sendRedirect(ctxPath+viewPage); //=>/mymvc + /abc.jsp
			}else {  //forward
				//뷰페이지로 포워드
				System.out.println("forward!\n");
				
				RequestDispatcher dispatcher
				=request.getRequestDispatcher(viewPage);
				dispatcher.forward(request, response);				
			}
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (InstantiationException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			e.printStackTrace();
		} catch (Throwable e) {
			e.printStackTrace();
		} 
	}
}
```













