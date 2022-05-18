# Servlet 매핑
Servlet의 요청을 Servlet의 이름이 아닌 다른 이름으로 요청을 받는 방법
1. Annotation @WebServlet Mapping
2. web.xml Mapping

### Annotation @WebServlet Mapping
```java
@WebServlet("/요청 URL")
public class [Servlet 이름] extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException {
	
	}
  
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException {
	
	}
}
```

### web.xml Mapping
해당 웹 어플리케이션의 web.xml에 파일에서
```xml
<servlet>
  <servlet-name>요청에 대한 서블릿 이름</servlet-name>
  <servlet-class>처리할 Servlet의 클래스 위치</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>해당 요청에 대한 서블릿 이름</servlet-name>
  <url-pattern>/요청이 들어올 URL</url-pattern>
</servlet-mapping>
```
