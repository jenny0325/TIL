# JSP 구조
예시 - HTML 문서를 생성하는 JSP 코드
```html
<!-- JSP 페이지에 대한 정보를 입력하는 설정 부분-->
<%@ page contentType = "text/html; charset=euc-kr" %>


<!-- HTML 문서를 생성하는 생성 부분-->
<html>
    <head>
        <title>HTML 문서의 제목</title>
    </head>
    <body>
        <%
        String bookTitle = "JSP 프로그래밍";
        String author = "최범균";
        %>
        <b><%= bookTitle %></b>(<%= author %>)입니다.
    </body>
</html>
```
- JSP 코드는 JSP 페이지 설정 부분(JSP 페이지에 대한 정보 입력), HTML 문서 생성 부분으로 나뉨
- JSP 페이지 설정 부분에 입력되는 정보:
  - JSP 페이지가 생성하는 문서의 타입(종류)
  - JSP 페이지에서 사용할 커스텀 태그 
  - JSP 페이지에서 사용할 자바 클래스 지정
- JSP 페이지 작성시 필요한 요소
  - JSP 페이지에 대한 정보 지정 
  - 웹 브라우저가 전송한 데이터 읽어오기
  - JSP 페이지에서 사용할 데이터를 생성하는 실행 코드 
  - 웹 브라우저에 문서 데이터를 전송하는 기능

# JSP의 구성 요소

- 디렉티브(Directive)
- 스크립트: 스크립트릿(Scriptlet), 표현식(Expression), 선언부(Declaration)
- 표현 언어(Expression Language)
- 기본 객체(Implicit Object)
- 정적인 데이터 
- 표준 액션 태그(Action Tag)
- 커스텀 태그(Custom Tag)와 표준 태그 라이브러리(JSTL)

## 디렉티브
- JSP 페이지에 대한 설정 정보를 지정할 때 사용
```html
<%@ 디렉티브이름 속성1="값1" 속성2="값2" ... %>
```
- JSP가 제공하는 디렉티브 종류:

|디렉티브	| 설명 |
|------|----|
|page | JSP 페이지에 대한 정보 지정, JSP가 생성하는 문서의 타입, 출력 버퍼의 크기, 에러 페이지 등 JSP 페이지에서 필요로 하는 정보 설정|
|taglib	| JSP 페이지에서 사용할 태그 라이브러리를 지정|
|include | JSP 페이지의 특정 영역에 다른 문서를 포함|

## page 디렉티브
- JSP 페이지에 대한 정보를 입력하기 위해 사용 
- ex) 생성 하는 문서 정보, 사용하는 자바 클래스, 세션 참여 여부, 출력 버퍼 존재 여부 등
```html
<%@ page contentType = "text/html; charset=utf-8" %>
<%@ page import="java.util.Date" %>
```

## 스크립트 요소
- JSP에서 문서의 내용을 동적으로 생성하기 위해 사용되는 요소
- 사용 예: 사용자가 폼에 입력한 정보를 데이터베이스에 저장, 데이터베이스로부터 게스글 목록을 읽어와 출력
- JSP 스크립트 요소의 종류:
  - 표현식(Expression): 값을 출력 
  - 스크립트릿(Scriptlet): 자바 코드를 실행 
  - 선언부(Declaration)
  
## 기본 객체
- JSP에서 웹 어플리케이션 프로그래밍에 필요한 기능을 제공하는 자바 객체 
- ex) request, response, session, application, page

# request 기본 객체
- request 기본 객체는 JSP 페이지에서 가장 많이 사용되는 기본 객체
- 웹 브라우저의 요청과 관련이 있다.
- 제공하는 기능 : 
  - 클라이언트(웹 브라우저)와 관련된 정보 읽기 기능
  - 서버와 관련된 정보 읽기 기능
  - 클라이언트가 전송한 요청 파라미터 읽기 기능
  - 클라이언트가 전송한 요청 헤더 읽기 기능
  - 클아이언트가 전송한 쿠키 읽기 기능
  - 속성 처리 기능

## 클라이언트 정보 및 서버 정보 읽기
|메서드 | 리턴 타입 | 설명|
|-----|---------|----|
|getRemoteAddr()| String| 웹 서버에 연결한 클라이언트의 IP 주소를 구함, ex) 게시판이나 방명록 등에서 자동으로 입력되는 글 작성자의 IP 주소 |
|getContentLength()|long|클라이언트가 전송한 요청 정보의 길이를 구함, 전송된 데이터의 길이를 알 수 없을 때는 -1 리턴|
|getCharacterEncoding()|String|클라이언트가 요청 정보를 전송할 때 사용한 캐릭터의 인코딩을 구함|
|getContentType()|String|클라이언트가 요청 정보를 전송할 때 사용한 컨텐츠의 타입을 구함|
|getProtocol()|String|클라이언트가 요청한 프로토콜을 구함|
|getMethod()|String|웹 브라우저가 정보를 전송할 때 사용한 방식을 구함|
|getRequestURI()|String|웹 브라우저가 요청한 URL에서 경로를 구함|
|getContextPath()|String|JSP 페이지가 속한 웹 어플리케이션의 컨텍스트 경로를 구함|
|getServerName()|String| 연결할 뗴 사용한 서버 이름을 구함|
|getServerPort()|int| 서버가 실행중인 포트 번호를 구함|

## 요청 파라미터 처리
### request 기본 객체의 요청 파라미터 관련 메서드
|메서드 | 리턴 타입 | 설명 |
|-----|---------|-----|
|getParameter(String name)| String | 이름이 name인 파라미터의 값을 구함, 없는 경우 null 리턴|
|getParameterValues(String name)| String[] |이름이 name인 모든 파라미터의 값을 배열로 구함, 없는 경우 null 리턴|
|getParameterNames()| java.util.Enumeration |웹 브라우저가 전송한 파라미터의 이름 목록을 구함|
|getParameterMap()| java.util.Map | 웹 브라우저가 전송한 파라미터의 맵을 구함, 맵은 <파라미터 이름, 값> 쌍으로 구성|

### GET 방식 전송과 POST 방식 전송
- 웹 브러우저는 GET 방식과 POST 방식 두 가지 방식 중 한 가지를 이용해서 파라미터를 전송함
- GET 방식 : 요청 URL에 파라미터를 붙여서 전송
  - URL의 경로 뒤에 물음표('?')와 함께 파라미터를 붙여 전송하는데 이를 쿼리 문자열이라고 함
  - & 기호로 구분하며, 파라미터의 이름과 값은 등호기호(=)로 구분
  - 파라미터 값은 RFC 2396 규약에 정의된 규칙에 따라 인코딩해서 전송
  ```text
  이름1=값1&이름2=값2&..이름n&값n 
  ```
### 요청 파라미터 인코딩
- 웹 브라우저는 웹 서버에 파라미터를 전송할 때 알맞은 캐릭터 셋을 이용해서 파라미터값을 인코딩 함
- 반대로 웹 서버는 알맞은 캐릭터 셋을 이용해서 웹 브라우저가 전송한 파라미터 데이터를 디코딩함
- POST 방식에서는 입력 폼을 보여주는 응답 화면이 사용하는 캐릭터 셋을 사용
- GET 방식 파라미터 전송 시 인코딩 결정 규칙

|GET 방식으로 전송 시 파라미터 전송 방법 | 인코딩 결정|
|--------------------------------|---------|
|\<a>태그의 링크 캐그에 쿼리 문자열 추가|웹 페이지 인코딩 사용|
|HTML 폼(FORM)의 method 속성값을 "GET"으로 지정새헛 폼을 전송| 웹 페이지 인코딩 사용|
|웹 브라우저에 주소에 직접 쿼리 문자열을 포함하는 URL 입력| 웹 브라우저마다 다름|

### 톰캣에서 GET 방식 파라미터를 위한 인코딩 처리하기
- 톰캣 8 버전에서 GET 방식으로 전달된 파라미터 값을 읽어올 때 사용하는 캐릭터 셋의 기본값은 UTF-8임
- UTF-8이 아닌 다른 캐릭터 셋을 이용해서 GET 방식의 파라미터를 전송하는 경우가 있다면, 다음과 같은 설정을 톰캣 8에 추가해주면 됨
  - server.xml 파일에서 <Connector>의 userBodyEncodingForURI 속성의 값을 true로 지정

### 요청 헤더 정보의 처리
- HTTP 프로토콜은 헿더 정보에 부가적인 정보를 담도록 하고 있음
- request 기본 객체가 제공하는 헤더 관련 메서드

|메서드|리턴 타입|설명|
|----|--------|---|
|getHeader(String name)|String|지정한 이름의 헤더 값을 구함|
|getHeaders(String name)|java.util.Enumeration|지정한 이름의 헤더 목록을 구함|
|getHeadersNames|java.util.Enumeration|모든 헤더의 이름을 구함|
|getIntHeader(String name)|int|지정한 헤더의 값을 정수 값으로 읽어옴|
|getDateHeader(String name)|long|지정한 헤더의 값을 시간 값으로 읽어옴|

# response 기본 객체
- response 기본 객체는 request 기본 객체와 반대의 기능을 수행
- response 기본 객체는 웹 브라우저에 보내는 응답 정보를 담는다.
- response 기본 객체가 제공하는 기능
  - 헤더 정보 입력
  - 리다이렉트 하기

## 웹 브라우저에 헤더 정보 전송하기
- response 기본 객체는 응답 정보에 헤더를 추가하는 기능을 제공
- response 기본 객체가 제공하는 헤더 관련 메서드

|메서드|설명|
|-----|---|
|addDateHeader(String name, long date)|name 헤더에 date를 추가|
|addHeader(String name, String value)|name 헤더에 value를 값으로 추가|
|addIntHeader(String name, int value)|name 헤더에 정수 값 value를 추가|
|setDateHeader(String name, long date)|name 헤더의 값을 date로 지정|
|setHeader(String name, String value)|name 헤더의 값을 value로 지정|
|setIntHeader(String name, int value)|name 헤더의 값을 정수 값 value로 지정|
|containsHeader(String name)|이름이 name인 헤더를 포함하고 있을 경우 true, 아닌경우 false 리턴|

## 웹 브라우저 캐시 제어를 위한 응답 헤더 입력
|응답 헤더|설명|
|-------|---|
|Cache-Control|HTTP 1.1버전에서 지원하는 헤더로서, 이 헤더의 값을 "no-cache"로 지정하면 웹 브라우저는 응답 결과를 캐시하지 않음|
|Pragma|HTTP 1.0 버전에서 지원하는 헤더로서, 이 헤더의 값을 "no-cache"로 지정하면 웹 브라우저는 응답 결과를 캐시에 저장하지 않음|
|Expires|HTTP 1.0 버전에서 지원하는 헤더로서, 응답 결과의 만료일을 지정, 만료일을 현재 시간보다 이전으로 설정|

## 리다이렉트를 이용해서 페이지 이동하기
- response 기본 객체에서 많이 사용되는 기능 중 하나
- 웹 서버가 웹 브라우저에게 다른 페이지로 이동하라고 응답하는 기능
- 다음의 메서드를 사용
```text
response.sedRedirect(String location)
```