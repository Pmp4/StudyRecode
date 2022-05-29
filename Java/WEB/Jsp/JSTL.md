# JSTL 개요
JSP 에는 XML 처럼 사용자가 태그를 <b>정의해서 사용하는 것</b>이 가능하다.

이것을 <b>사용자 정의 태그</b>(커스텀 태그)라고 하는데, 이들 중 자주 사용하는 것을 표준으로 만들어 놓은 것이 ```JSTL(JSP Standard Tag Libary)```

- 자주 사용하는 커스텀 태그(Custom Tag)의 표준
- 많은 JSP 어플리케이션을 간단한 태그로 캡슐화
- 액션태그는 일종의 시스템이 제공하는 커스텀 태그
  - ```<jsp:useBean id="vo" class="study.BoardVO"/>``` 자바빈 객체 생성
  - ```<jsp:include page="~~"/>``` include 액션태그
  - ```<jsp:forward page="~~"/>``` forward 액션태그

또한 JSTL을 사용하여 Jsp 페이지의 로직부분을 태그로 대체시켜 간략화와 가독성을 향상 시킬 수 있다.

## JSTL(JSP Standard Tag Libary)을 사용하기 위한 환경 설정
1. [page](https://mvnrepository.com/artifact/javax.servlet/jstl/1.2) 접속 후, 'Files' => 'jar' 클릭
2. 다운로드한 해당 jar 파일을 프로젝트의 ```/WEB-INF/lib```에 복사
3. JSTL 사용 시 라이브러리 지정 ```<%@ taglib prefix="~" uri="~~" %>```

## JSTL이 제공하는 태그의 종류
|라이브러리|URI|Prefix(접두어)|사용 예시|
|--|--|--|--|
|<b>Core (코어)</b>|http://java.sun.com/jsp/jstl/core|c|<c:tagname...>|
|XML processing|http://java.sun.com/jsp/jstl/xml|x|<x:tagname...>|
|<b>I18N capable formatting</b>|http://java.sun.com/jsp/jstl/fmt|fmt|<fmt:tagname...>|
|Database access(SQL)|http://java.sun.com/jsp/jstl/sql|sql|<sql:tagname...>|
|<b>Functions(함수)</b>|http://java.sun.com/jsp/jstl/functions|fn|fn:functionName(...)|

### JSTL core
변수선언, 삭제 등 변수와 관련된 작업과 if문, for문 등과 같은 제어문, URL 처리 등에 사용

#### JSTL core 의 기능별 분류 
- 표현언어지원기능
  - <c:catch>, <c:out>, <c:remove>, <c:set>
- 흐름제어기능
  - <c:choose>, <c:when>,<c:otherwise> 
  - <c:forEach>, <c:forTokens>, <c:if>
- URL 관리 기능
  - <c:import>, <c:param>
  - <c:redirect>, <c:param>
  - <c:url>, <c:param>

