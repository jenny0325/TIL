# 서블릿(Servlet)이란?
- 서블릿은 자바로 웹 어플리케이션을 개발할 수 있도록 하기 위해 만들어진 표준
  - 응답과 요청을 위한 객체들을(HttpServletRequest, HttpServletResponse, HttpServlet) 제공한다.
  - JSP 표준이 나오기 전에 만들어짐
- 서블릿의 개발 과정
  - 서블릿 규약에 따라 자바 코드 작성
  - 자바 코드를 컴파일해서 클래스 파일 생성
  - 클래스 파일을 /WEB-INF/classes 폴더에 패키지에 알맞게 위치시킴
  - web.xml 파일에 서블릿 클래스를 설정
  - 톰캣 등의 컨테이너를 실행
  - 웹 브라우저에서 확인
- MVC 패턴을 지원하는 프레임워크를 만들어야 하는 경우 서블릿으로 기반 코드를 개발하는 경우가 많음

# 서블릿 구현
```java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class NowServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html; charset=utf-8");
        
        PrintWriter out = response.getWriter();
        out.println("<html>");
        out.println("<head><title>현재시간</title></head>");
        out.println("<body>");
        out.println("현재 시간은");
        out.println(new Date());
        out.println("입니다.");
        out.println("</body></html>");
    }
}
```
- HttpServlet 클래스를 상속받아야 서블릿으로 동작함 
- 처리하고자하는 HTTP 방식(method)에 따라 알맞은 메서드를 재정의
  - 위에서는 doGet() 메서드를 재정의
  - doGet() 메서드가 갖는 HttpServletRequest와 HttpServletResponse 두 파라미터는 각각 JSP의 request 기본 객체와 response 기본 객체에 해당함

- 재정의한 메서드는 request를 기용해서 웹 브라우저의 요청 정보를 읽어오거나, response를 이용해서 응답을 전송함
  - 응답을 전송하려면 response.setContentType() 메서드를 이용하여 응답 컨텐츠 타입을 지정해야함
  - setContentType() 메서드에 전달되는 값은 JSP에서 page 디렉티브의 contentType 속성값과 동일함

- 웹 브라우저에 데이터를 전송하려면(응답 컨텐츠 타입 지정 후), response.getWriter()로 문자열 데이터를 출력할 수 있는 PrintWriter를 구함
  - PrintWriter는 println() 메서드를 제공하며, 이 메서드를 이용해서 전송할 응답 데이터를 전달함



# web.xml로 매핑
- WEB-INF 폴더의 web.xml 파일에서 서블릿 클래스를 등록 가능
- 예시 - NowServlet 클래스를 web.xml에 등록
```xml
<?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
        version="3.1">
        <servlet>
            <servlet-name>now</servlet-name>
            <servlet-class>ch17.NowServlet</servlet-class>
        </servlet>
        
        <servlet-mapping>
            <servlet-name>now</servlet-name>
            <url-pattern>/now</url-pattern>
        </servlet-mapping>
    </web-app>
```
- 서블릿을 등록하려면 두 가지 설정이 필요
  - 서블릿으로 사용할 클래스
  - 서블릿과 URL 간의 매핑
- servlet 태그를 이용해서 서블릿 클래스를 등록  
    - servlet-name 태그를 이용해서 서블릿을 참조할 때 사용할 이름 등록
    - servlet-class 태그를 이용해서 서블릿 클래스의 완전한 이름 입력
- servlet-mapping 태그를 이용해서 서블릿이 어떤 URL을 처리할지에 대한 매핑 정보 등록
  - servlet-name 태그를 이용해서 서블릿 이름 등록
  - url-pattern 태그를 이용해서 매핑할 URL 패턴 지정
- 위와 같이 NowServlet을 등록하고 톰캣을 실행한 후 매핑한 주소를 웹 브라우저에 입력하면, 다음과 같은 결과를 볼 수 있음
```text
<결과>
현재 시간은 Mon Jan 16 09:34:32 KST 2023 입니다.
```
- <url-pattern>은 한 번 이상 사용 가능하고, 이 경우 각각의 URL 패턴에 해당 서블릿을 매핑
```xml
<servlet-mapping>
    <servlet-name>now</servlet-name>
    <url-pattern>/now</url-pattern>
    <url-pattern>/now2</url-pattern>
</servlet-mapping>
```
- <url-pattern>은 웹 어플리케이션 경로를 제외한 나머지 경로를 기준으로 적용
  - 아래의 경우 실제 매핑되는 URL은 <url-pattern>의 /now와 매핑되는 웹 어플리케이션 경로를 포함한 /ch17/now가 됨

```xml
<servlet-mapping>
    <servlet-name>now</servlet-name>
    <url-pattern>/now</url-pattern>
</servlet-mapping>

```
# 애노테이션으로 매핑
- 서블릿 2.5 버전까지는 web.xml 파일에 서블릿으로 등록해야 서블릿 클래스를 사용할 수 있었지만, 서블릿 3.0 버전부터 @WebServlet 애노테이션을 사용하면 파일에 따로 등록 없이 서블릿이 등록됨
- 예시 - @WebServlet을 통한 서블릿 등록
```java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet(urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html; charset=utf-8");
        
        PrintWriter out = response.getWriter();
        out.println("<html>");
        out.println("<head><title>인사</title></head>");
        out.println("<body>");
        out.println("안녕하세요, ");
        out.println(request.getParameter("name"));
        out.println("님");
        out.println("</body></html>");
    }
}
```
- @WebServlet 애노테이션의 urlPatterns 속성은 해당 서블릿과 매핑될 URL 패턴을 지정할 때 사용 
- 두 개 이상의 URL 패턴을 처리하려면, urlPatterns 속성값으로 배열을 전달
```java
@WebServlet(urlPattern = {"/hello","/hello1"})
```
- @WebServlet 애노테이션을 사용할 경우 서블릿이 처리해야 할 URL 패턴이 변경될 때마다 자바 소스 코드의 urlPatterns 속성값을 변경하고 다시 컴파일해야 하지만,
web.xml 파일을 사용하면 URL 경로가 바뀔 경우 web.xml 파일만 변경하면 됨
  - 서블릿의 용도에 따라서 @WebServlet 애노테이션, web.xml 설정 사용을 결정하기
- @WebServlet 애너테이션이 적용된 서블릿을 web.xml 파일에 등록하면, 두 설정에 적용된 URL 패턴이 모두 매핑되고, 각각의 객체가 생성됨


# HTTP 각 방식별 구현 메서드
- HTTP는 GET, POST, PATCH, HEAD, PUT, DELETE 방식을 지원
  - 일반적으로 웹에서 사용되며 웹 브라우저가 지원하는 방식은 GET, POST
- HttpServlet은 HTTP 각 방식에 따라 알맞은 메서드를 이용해서 구현하도록 정의
  - GET 방식은 doGet() 메서드를 이용해서 처리
  - POST 방식의 경우 doPost() 메서드를 이용해서 처리
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
```

# 서블릿 로딩과 초기화
- 서블릿 컨테이너는 서블릿을 최초 요청할 때 서블릿 객체를 생성하고, 이후 요청이 오면 앞서 생성한 서블릿 객체를 그대로 사용함

![img](https://user-images.githubusercontent.com/89081374/212702038-c56df330-49d6-4bb0-9a20-a3797e014c6e.png)
- 서블릿 컨테이너는 서블릿을 초기화하기 위해 ServletConfig 파라미터를 갖는 init() 메서드를 실행함
- init() 메서드의 기본 구현:
```java
// GenericServlet 구현
public void init(ServletConfig config) throws ServletException {
  this.config = config;
  this.init();
}

public void init() throws ServletException {
    
}
```
- init(ServletConfig) 메서드는 파라미터가 없는 init() 메서드를 호출하므로, 초기화가 필요한 서블릿은 파라미터가 없는 init() 메서드를 재정의한다.
```java
public class DBCPInit extends HttpServlet {

  @Override
  public void init() throws ServletException {  // 서블릿을 초기화할 때 사용
    loadJDBCDriver();
    initConnectionPool();
  }

}
```
- 초기화 작업은 상대적으로 시간이 오래 걸리므로, 처음 서블릿을 사용하는 시점보다 웹 컨테이너를 처음 구동하는 시점에 초기화를 진행하는 것이 좋고, 이를 위한 <load-on-startup> 태그가 존재함
```xml
<servlet>
    <servlet-name>DBCPInit</servlet-name>
    <servlet-class>jdbc.DBCPInit</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
```
<img width="523"  src="https://user-images.githubusercontent.com/89081374/212702434-fad8a650-5cc6-43d2-a464-2131882f1443.png">
- load-on-startup 태그의 값은 로딩 순서를 의미하고, 값을 기준으로 오름차순으로 서블릿을 로딩함
- @WebServlet 태그를 사용하는 경우 loadOnStartup 속성을 이용해 로딩 값 지정

```java
@WebServlet(urlPatterns = "/hello", loadOnStartup = 1)
public class InitServlet extends HttpServlet{
...
}
```
# 초기화 파라미터
- 서블릿은 web.xml의 <init-param> 태그를 이용해서 서블릿을 초기화할 때 필요한 값을 전달하는 방법을 제공함
```xml
<servlet>
    <init-param>
        <param-name>jdbcdrvier</param-name>
        <param-value>com.mysql.jdbc.Driver</param-value>
    </init-param>
  <servlet>
```
- init-param 태그의 자식 태그
  - param-name 태그: 초기화 파라미터의 이름을 지정
  - param-value 태그: 초기화 파라미터의 값을 지정
- 서블릿 클래스에서 초기화 파라미터에 접근하려면 getInitParamter() 메서드를 사용함
  - 이때, 초기화 파라미터가 존재하지 않으면 null을 리턴
```java
String driverClass = getInitParameter("jdbcdriver");
```
- @WebServlet 애노테이션으로 매핑한 경우 초기화 파라미터를 전달하려면, initParams 속성의 값으로 @WebInitParam 애노테이션 목록을 전달함
```java
@WebServlet(urlPatterns = {"/hello", "/hello1"},
    initParams = {
        @WebInitParam(name="greeting", value="Hello"),
        @WebInitParam(name="title", value="제목")
    }
}
```


# URL 패턴 매핑 규칙
- url-pattern 태그와 urlPatterns 속성에서 사용할 수 있는 URL 패턴이 어떻게 서블릿과 매핑되는가?
- URL 패턴이 서블릿을 매핑하는 규칙:
  - '/'로 시작하고 '/*'로 끝나는 url-pattern은 경로 매핑을 위해서 사용
  - '*.'로 시작하는 url-pattern은 확장자에 대한 매핑을 할 때 사용
  - 오직 '/'만 포함하는 경우 어플리케이션의 기본 서블릿으로 매핑
  - 이 규칙 외, 나머지 다른 문자열은 정확한 매핑을 위해 사용
- 예시 - URL 패턴에 따른 처리 서블릿

|URL 패턴|매핑 서블릿|
|-------|--------|
|/foo/bar/*|	servlet1|
|/baz/*	|servlet2|
|/catalog|	servlet3|
|*.bop|	servlet4|

- 위와 같은 URL 패턴을 설정한 경우 실제 요청 경로에 따라서 요청을 처리하는 서블릿

|요청 경로	|일치 URL 패턴|	요청 처리 서블릿|
|-----------|-----------|---------------|
|/foo/bar/index.html|	/foo/bar/*|	servlet1|
|/foo/bar/index.bop|	/foo/bar/*|	servlet1|
|/baz	|/baz/*	|servlet2|
|/baz/index.html|	/baz/*	|servlet2|
|/catalog	|/catalog	|servlet3|
|/catalog/racecar.bop	|*.bop|	servlet4|
|/index.bop	|*.bop|	servlet4|
