💡 Java WEB Application 기본 💡
=================================
# 🚧 웹 어플리케이션이 동작하기 위한 구조

웹 클라이언트의 Request(요청)을 받기 위해선 ```웹 서버```가 필요하고, <b>JSP 문서</b> 요청일 시 웹 서버 안에서 동작하는 ```웹 컨테이너```에 해당 문서를 넘긴다.

웹 컨테이너는 JSP 문서를 Java 형식인 ```Servlet(서블릿)```으로 변환하고 컴파일을 하게된다. (JSP문서를 해석하고 가공)

컴파일된 Servlet은 최종적으로 웹 브라우저에 Response 되어져 사용자는 응답 결과를 보게된다.

|웹 클라이언트(사용자)| 
|:--:|
|⬇️|
|웹 서버|
|⬇️|
|웹 컨테이너(JSP문서 해석)|
|⬇️|
|헤석된 HTML문서로 결과 response(응답)|

> 클라이언트(사용자) : HTML, CSS, JAVASCRIPT<br>
> 서버단 : JSP, DB

## 🆘 요청형식
웹 어플리케이션의 요청에는 인터넷 주소가 필요하고,
```도메인```
```어플리케이션```
```소스페이지```
이러한 구조로 이뤄져 있다.

> 예) http://202.131.30.11:80/app/index.jsp?id=hong&age=20

```202.131.30.11:80``` 도메인(아이피)

```/app``` 어플리케이션

```/index.jsp?id=hong&age=20``` 소스페이지 ('?' 뒤는 파라미터)

> [프로토콜]://[호스트][:포트][경로][파일명][.확장자][쿼리 문자열]

# 🧑‍💻 Java WEB Application 개발
자바에서의 웹 개발 방식에는 크게 두 가지가 있다.
- 스크립트 방식의 ```JSP```
- 실행코드 방식의 ```Servlet(서블릿)```

위 방식으로 만들어진 코드를 보관할 디렉토리가 필요한데, 그 디렉토리의 명칭을 ```웹 어플리케이션``` ```웹 사이트``` ```웹 프로젝트``` ```context```라고 한다.

## 웹 어플리케이션 디렉토리 생성 방법
### 1. ```[웹 컨테이너 디렉토리]/webapps/``` 하위 폴더를 추가하여 생성하는 방법
1. 하위 디렉토리로 ```웹 어플리케이션``` 이름의 폴더를 생성하고 .jsp문서를 생성한다.<br>
2. 해당 폴더에 ```WEB-INF``` 폴더를 생성하고, 그 안에 classes, src 폴더를 생성한다.<br>
3. 톰캣 재부팅 후 ```localhost:설정한포트/웹 어플리케이션/~~.jsp```로 웹 브라우저로 실행
> 웹 어플리케이션을 context path라고 부른다<br>
> ```webapps``` 디렉토리를 하나의 웹 어플리케이션에 해당하고, 접근할 때의 경로로 시작된다.

### 2. ```server.xml``` 파일에 ```context```를  추가하여 생성하는 방법
톰캣의 webapps 폴더에 웹 어플리케이션을 생성하는 것이 아닌, 다른 경로에 만들고자 할 때 사용된다.

1. ```톰캣 디렉토리/conf/server.xml``` 실행
2. ```<host></host>``` 태그 안 마지막에 ```<Context path="/웹 어플리케이션 이름" docBase="해당 문서가 있는 절대위치"/>``` 를 추가
<br>

## 웹 프로그래밍
### Servlet (서블릿)
자바언어를 개발한 Sun Microsystems(현 Oracle)에서 웹 개발을 위해 만든 표준<br>
서블릿을 만들기 위해서는 자바코드를 작성하고, 코드를 컴파일해서 클래스 파일을 만들게 된다.
```java
@WebServlet("/서블릿 경로명") //서블릿 클래스를 해당 어플리케이션 이름으로 맵핑(주소창에 입력될 때 )
public class 서블릿이름 extends HttpServlet {

  //doGet()은 request(요청)이 get일 경우 웹 컨테이너의 의해 호출된다.
  @Override
  protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException{
    response.setContentType("text/html; charset=UTF-8"); //response(응답)문서에 대한 설정
    PrintWrite out = response.getWrite(); //출력스트림
    
    out.print("<html><head><title></title></head></html>"); //출력스트림을 사용하여 응답문서를 작성할 수 있다.
  }
  
  @Override
  protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException{
    response.setContentType("text/html; charset=UTF-8");
    
    /*요청 방식이 post일 경우, request(요청) 파라미터의 대한 인코딩 설정*/
    request.setCharacterEncoding("UTF-8");
  }
}
```
<br>

### JSP
자바 기반으로 하는 ```Server Side 스크립트 언어```

웹 컨테이너의 의해 JSP가 번역되면 최종 결과물로 Servlet이 만들어진다.
- HTML코드에 Java코드를 넣어 동적인 웹페이지를 생성하는 웹 어플리케이션 도구
  - JSP를 통해 정적인 HTML문서와 동적으로 생성된 HTTP request(요청) 파라미터를 혼합하여 사용할 수 있다. 
  - 즉, 하나의 페이지를 사용자가 요청한 값에 따라 다르게 보여질 수 있는 것이다.
- Servlet 기술의 확장
  - 기존 Servlet은 ```실행코드방식```으로 개발 효율성 문제가 존재했지만, JSP는 스크립트 언어로 필요한 부분만 수정할 수 있다.
  - Servlet의 모든 기능 + 추가적인 기능

#### 문법
##### 1. JSP Expression(표현식)
> ```<%= Expression %>```
> JSP Expression은 Servlet의 출력으로 사용된다.

##### 2. JSP Scriptlet(스크립트릿)
```<% code fragment %>```
> 간단한 값이 아닌 좀 더 복잡한 것을 수행할 때 사용된다.<br>
> 임의의 Java코드를 사용할 수 있다.

##### 3. JSP Comment
```<%-- comment --%>```
> 일종의 주석

##### 4. JSP Directive(지시어)
```<%@ directive %>```
> JSP







