# FileUpload(파일 업로드)
웹 브라우저를 통해 파일 업로드를 할 경우에는
```jsp
<form name="frm1" method="post" action="upload_ok.jsp" enctype="multipart/form-data">
  파일 업로드 : <input type="file" name="fileName" size="30"><br>
  <input type = "submit" value="등록"><br>
</form>
```
이러한 형식으로 요청해야한다.

- form
  - method = <b>post</b>
  - enctype = <b>multipart/form-data</b>
- input
  - type = <b>file</b>
<br>

> <b>post 방식일 경우 2가지 인코딩</b>
> 1. application/x-www-form-urlencoded : 디폴트, 파일 이름만 전송됨
> 2. multipart/form-data : 파일 이름과 함께 파일 데이터가 전송됨
<br>

```enctype=“multipart/form-data”``` 로 전송한 폼에 담겨진 파라미터들에 대한 <b>이름</b>과 <b>값</b>을 가져오고,
```<input type=“file”>```로 지정된 파일을 <b>서버상</b>의 한 폴더에 <b>업로드</b>하기 위해서 특별한 컴포넌트가 필요함

다양한 컴포넌트 중 <a href="www.servlets.com">www.servlets.com</a>에서 제공하는 cos.jar 라이브러리 중, ```MultipartRequest``` 컴포넌트를 사용하겠다.

### MultipartRequest 클래스
MultipartRequest 클래스를 사용해서 파일 업로드 수행

<b>생성자</b>
```java
MultipartRequest(javax.servlet.http.HttpServletRequest request,
  java.lang.String saveDirectory,
  int maxPostSize,
  java.lang.String encoding,
  FileRenamePolicy policy
)
```

### 요청을 받은 jsp에서의 처리
