- JSP 웹 어플리케이션의 구조는 모델 1 구조와 모델 2 구조로 나뉨
- JSP에서 모든 로직과 출력을 처리하느냐 JSP에서는 출력만 처리하느냐에 따라 모델 1, 2 구조로 구분
- MVC 패턴을 이용해서 웹 어플리케이션을 구현할 때 모델 2 구조를 사용함

# Model 1 아키텍처

- JSP와 JavaBeans만 사용하여 웹을 개발하는 구조
- 90년대 말부터 2000년대 초까지 자바 기반의 웹 애플리케이션 개발에 사용됐던 아키텍처
  ![img](https://user-images.githubusercontent.com/89081374/212838194-34a8646b-7919-4686-bd53-dae9e5ef01fe.png)
- 웹 브라우저의 요청을 JSP가 직접 처리
- 웹 브라우저의 요청을 받은 JSP는 자바빈이나 서비스 클래스를 사용해서 작업을 처리하고, 결과를 클라이언트에 출력
- Model 1 아키텍처에서 Model의 기능은 JavaBeans에 의해 이루어짐
- 모델(Model)은 데이터베이스 연동 로직을 제공하면서, DB에 검색한 데이터가 저장되는 자바 객체를 말함
- JavaBeans의 Bean은 자바에서 객체를 의미하는 용어이고, JavaBeans는 데이터베이스 연동에 사용되는 자바 객체를 의미함
- Model 1 아키텍처에서는 JSP 파일이 가장 중요한 역할을 수행함
    - JSP가 Controller와 View 기능을 모두 처리
- Controller: JSP에서 사용자의 요청 처리와 관련된 코드를 의미함
- View: JSP 파일에서 Model을 사용하여 검색한 데이터를 사용자가 원하는 화면으로 제공하는 기능
    - JSP에서는 HTML과 CSS가 이것을 담당

- Model1 구조의 단점: Model1 구조는 JSP 파일에서 Cotroller, View 기능을 모두 처리하므로, JSP 파일에 자바 코드와 마크업 관련 코드(HTML, CSS)가 뒤섞여 있어서,
    - 역할 구분이 명확하지 않고
    - 디버깅과 유지보수가 어려움
- 이러한 이유로 Model 1의 문제점을 보완한 Model 2, MVC(Model view Controller) 아키텍처가 등장함

# Model 2 아키텍처

- Model 1 의 문제점을 극복하기 위해 고안됨
- Model 1 구조와 달리 웹 브라우저의 모든 요청을 단일 진입점 즉, 하나의 서블릿에서 처리함
- 서블릿은 웹 브라우저의 요청을 알맞게 처리한 후, 결과를 보여줄 JSP 페이지를 선택하여 포워딩함
    - 모델 2 구조의 이러한 특징 때문에 MVC 패턴을 이용해서 웹 어플리케이션을 구현할 때 모델 2 구조를 사용함
- 포워딩을 통해 요청 흐름을 받은 JSP 페이지는 결과 화면을 클라이언트에 전송
- Controller가 도입
    - 기존에 JSP가 담당했던 Controller 로직이 별도의 Controller 기능의 서블릿으로 옮겨짐
      ![img](https://user-images.githubusercontent.com/89081374/213921581-f9555b74-82b5-49aa-bc73-bdb127bcc667.png)
- Controller 로직이 사라진 JSP에는 View와 관련된 디자인만 남게 되어 디자이너는 JSP 파일을 관리하고, 자바 개발자는 Controller와 Model만 관리할 수 있게됨

|기능|구성 요소|개발 주체|
|---|-------|------|
|Model|VO,DAO 클래스|자바 개발자|
|View|JSP 페이지|웹 디자이너|
|Controller|Servlet 클래스|자바 개발자 또는 MVC 프레임워크|

# MVC 패턴

- MVC(Model View Controller) 패턴은 모델, 뷰, 컨트롤러 세 부분으로 구성
    - 모델: 비즈니스 영역의 로직을 처리
    - 뷰: 비즈니스 영역에 대한 프레젠테이션 뷰(사용자가 보게 될 결과 화면)
    - 컨트롤러: 사용자의 입력 처리와 흐름 제어 담당
      ![img](https://user-images.githubusercontent.com/89081374/213921727-196b317e-c62e-4764-89c2-155505e8419e.png)
- 사용자는 원하는 기능을 처리하기 위한 모든 요청을 컨트롤러에 보냄
- 모델은 비즈니스와 관련된 기능을 제공
- 컨트롤러는 모델을 이용해서 사용자의 요청을 처리하고, 모델을 사용하여 알맞은 비즈니스 로직을 수행한 후 사용자에게 보여줄 뷰를 선택함
- 선택된 뷰는 사용자에게 알맞은 결과 화면을 보여줌
    - 뷰가 사용자에게 결과 화면을 보여줄 때 필요한 데이터는 컨트롤러를 통해서 전달받음

### MVC 패턴의 핵심

- 비즈니스 로직을 처리하는 모델과 결과 화면을 보여주는 뷰를 분리
- 어플리케이션의 흐름 제어나 사용자의 처리 요청은 컨트롤러에 집중

### MVC 패턴의 장점

- 유지보수 작업이 쉬워짐
- 어플리케이션을 쉽게 확장 가능
- 모델과 뷰가 분리되므로 모델의 내부 로직이 변경되어도 뷰는 영향을 받지 않음
- 뷰와 모델이 직접 연결되지 않으므로 내부 구현 로직에 상관없이 뷰를 변경 가능
- 컨트롤러는 비즈니스 로직에는 포함되지 않지만, 전체 웹 어플리케이션에 일괄적으로 적용되는 기능(예: 사용자 인증) 처리함
- 웹 어플리케이션의 흐름 제어나 보안 설정이 변경되면 컨트롤러만 변경하고, 새로운 타입의 사용자(예: 새로운 모바일 기기)가 추가되는 경우 컨트롤러나 모델에 상관없이 새로운 뷰를 추가

|모델|장점|단점|
|---|---|---|
|모델 1|배우기 쉬움, 자바 언어를 몰라도 어느 정도 구현 가능, 기능과 JSP가 직관적으로 연결    |- 로직 코드와 뷰 코드가 혼합되어 JSP 코드가 복잡해짐, 뷰 변경 시 논리 코드의 빈번한 복사가 발생 -> 코드 중복이 발생하기 쉬움 -> 유지보수가 힘들어짐
|모델 2|로직 코드와 뷰 코드를 분리해서 유지보수가 쉬움, 컨트롤러 서블릿에서 권한 검사나 인증과 같은 공통 기능 처리가 가능 ,확장이 용이함|자바 언어에 친숙하지 않으면 접근하기가 쉽지 않음, 작업량이 많음|

# MVC 패턴과 모델 2 구조의 매핑

- 모델 2 구조와 MVC 패턴을 완별하게 일치함
    - 컨트롤러 == 서블릿
    - 모델 == 로직 처리 클래스, 자바빈
    - 뷰 == JSP
    - 사용자 == 웹 브라우저 또는 스마트폰 등의 기기

## MVC의 컨트롤러: 서블릿

- 모델 2 구조의 서블릿은 MVC 패턴의 컨트롤러 역할
- 서블릿은 웹 브라우저의 요청과 웹 어플리케이션의 전체적인 흐름을 제어함

![img](https://user-images.githubusercontent.com/89081374/213922018-5b032eee-fa78-49da-86e1-4aafac0758c8.png)

- 웹 브라우저의 결과를 보여줄 JSP 페이지는 컨트롤러 서블릿이 선택하며, 요청 처리 결과는 request 또는 session에 저장해서 뷰 역할을 하는 JSP 페이지에 전달함

## MVC의 뷰: JSP

- 모델 2 구조에서 JSP는 뷰 역할을 담당함
- 컨트롤러에서 request나 session 기본 객체에 저장한 데이터를 사용하여 웹 브라우저에 알맞은 결과 출력
- JSP는 웹 브라우저가 요청한 결과를 보여주는 프레젠테이션 역할, 웹 브라우저의 요청을 컨트롤러에 전달해주는 매개체 역할을 함

## MVC의 모델

- 컨트롤러는 서블릿으로, 뷰는 JSP로 구현하지만 모델은 무엇으로 구현한다는 규칙이 존재하지 않고, 비즈니스 로직을 처리해주면 모델이 될 수 있음
- 모델이 제공해야 하는 기능: 웹 브라우저의 요청을 처리하는 데 필요한 기능
- ex) 계좌 이체 기능 요청 -> 모델은 계좌 이체를 시켜주는 기능을 제공하고, 컨트롤러는 웹 브라우저의 요청이 들어오는 경우 모델의 계좌 이체 기능을 사용
  ![img](https://user-images.githubusercontent.com/89081374/213922095-d7eef0ed-778d-4b04-8083-c3af6c5c0d38.png)

### 모델 2 구조의 구현 방법: 기본 MVC 패턴 구현 기법

- 예시 - 서블릿은 화면에 출력할 메시지를 생성해서 JSP에 전달, JSP는 서블릿으로부터 전달받은 메시지를 화면에 출력

```java
public class SimpleController extends HttpServlet {

    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        processRequest(request, response);
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        processRequest(request, response);
    }

    private void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 2단계, 요청 파악
        // request 객체로부터 사용자의 요청을 파악하는 코드
        String type = request.getParameter("type");

        // 3단계, 요청한 기능을 수행한다.
        // 사용자의 요청에 따라 알맞은 코드
        Object resultObject = null;
        if (type == null || type.equals("greeting")) {
            resultObject = "안녕하세요";
        } else if (type.equals("date")) {
            resultObject = new java.util.Date();
        } else {
            resultObject = "Invalid Type";
        }

        // 4단계, request나 session에 처리 결과를 저장
        request.setAttribute("result", resultObject);

        // 5단계, RequestDispatcher를 사용하여 알맞은 뷰로 포워딩
        RequestDispatcher dispatcher = request.getRequestDispatcher("/simpleView.jsp");
        dispatcher.forward(request, response);
    }
}
```

- 서블릿 클래스는 컨트롤러의 역할을 함
- 웹 브라우저의 요청 방식(GET, POST)에 상관없이 1단계에서 processRequest() 메서드로 웹 브라우저의 요청을 전달
- processRequest() 메서드는 나머지 네 과정을 차례로 실행
    - 2단계. request 객체를 사용해서 사용자가 요청한 기능을 파악
    - 3단계. 사용자의 요청에 따라서 필요한 기능을 수행한 뒤 결과값 생성
    - 4단계. request 또는 session 객체의 속성에 결과값 저장. 저장한 결과값은 뷰의 역할을 하는 JSP 페이지에서 사용.
    - 5단계. RequestDispatcher.foward() 메서드를 사용해서 결과를 생성할 뷰로 이동
- 서블릿 클래스를 실행하려면 web.xml 파일에 요청 URL과 서블릿 간의 매핑을 입력해야함

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">
    <servlet>
        <servlet-name>SimpleController</servlet-name>
        <servlet-class>mvc.simple.SimpleController</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>SimpleController</servlet-name>
        <url-pattern>/simple</url-pattern>
    </servlet-mapping>
</web-app>
```

- 위 예시가 실행되는 순서:

![img](https://user-images.githubusercontent.com/89081374/213922253-166f0e03-84a3-49b7-86b5-08e585c7668e.png)

## 커맨드(Command) 패턴 기반의 코드

- 각 명령어에 해당하는 로직 처리 코드를 별도 클래스로 작성하는 방법
    - 즉, 하나의 명령어를 하나의 클래스에서 처리하는 패턴
- 각 명령어에 따른 로직 처리 코드를 모두 컨트롤러 서블릿에서 처리하면, 서블릿 코드가 복잡해지기 때문에 해당 패턴을 사용함

```java
String command=request.getParameter("cmd");
        CommandHandler handler=null;

        if(command==null){
        handler=new NullHandler();
        }else if(command.equals("BoardList")){
        handler=new BoardListHandler();
        }else if(command.equals("BoardWriteForm")){
        handler=new BoardWriteFormHandler();
        }

        String viewPage=handler.process(request,response);

        RequestDispatcher dispatcher=request.getRequestDispatcher(viewPage);
        dispatcher.forward(request,response);
```

- 컨트롤러 서블릿은 명령어에 해당하는 CommandHandler 인스턴스를 생성하고, 실제 로직의 처리는 생성한 CommandHandler 인스턴스에서 실행되는 구조

![img](https://user-images.githubusercontent.com/89081374/213922333-3ec69343-cd9b-4e16-9584-6b02828710f7.png)

- 커맨드 패턴에서 명령어를 처리하느 클래스는 공통 인터페이스를 상속해서 구현
  ![img](https://user-images.githubusercontent.com/89081374/213922366-ba340818-b627-4892-b7ff-ad2aae692fa3.jpeg)

```java
package mvc.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface CommandHandler {
    public String process(HttpServletRequest req, HttpServletResponse res) throws Exception;
}
```

- 명령어를 처리하는 핸들러 클래스는 CommandHandler 인터페이스를 구현하고, 핸들러 클래스의 process() 메서드는 세 가지 작업을 처리함

```java
public class SomeHandler implements CommandHandler {

    public String process(HttpServletRequest request, HttpServletResponse response) {
        // 1. 명령어와 관련된 비즈니스 로직 처리
        ...
        // 2. 뷰 페이지에서 사용할 정보 저장
        request.setAttribute("somValue", value);
        ...
        // 3. 뷰 페이지의 URI 리턴
        return "/view/someView.jsp";
    }
}
```

## 설정 파일에 커맨드와 클래스의 관계 명시하기

- 아래 코드는 로직 처리 코드를 컨트롤러 서블릿에서 핸들러 클래스로 옮겼지만, 컨트롤러 서블릿은 명령어에 따른 알맞은 처리를 하기 위해 알맞은 처리를 하기 위해 중첩된 if-else 구문을 사용
- 단점: 새로운 명령어가 추가되면 컨트롤러 서블릿 클래스의 코드를 직접 변경해야함

```java
String command=request.getParameter("cmd");
        CommandHandler handler=null;

        if(command==null){
        handler=new NullHandler();
        }else if(command.equals("BoardList")){
        handler=new BoardListHandler();
        }else if(command.equals("BoardWriteForm")){
        handler=new BoardWriteFormHandler();
        }
```

- 위 단점을 해결하는 방법: <명령어, 핸들러 클래스>의 매핑 정보를 설정 파일에 저장하기

```java
BoardList=mvc.command.BoardListHandler
        BoardWriteForm=mvc.command.BoardWriteFormHandler
        ...
```

- 컨트롤러 서블릿은 init() 메서드를 사용해서 서블릿을 생성하고 초기화할 때, 명령어와 핸들러 클래스의 매핑 정보를 읽어와서 명령어에 해당하는 핸들러 클래스 객체를 미리 생성해두었다가 process()
  메스드에서 사용하면 된다.

```java
import java.io.FileReader;
import java.io.IOException;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Properties;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import mvc.command.CommandHandler;
import mvc.command.NullHandler;

public class ControllerUsingFile extends HttpServlet {

    // <커맨드, 핸들러인스턴스> 매핑 정보 저장
    private Map<String, CommandHandler> commandHandlerMap = new HashMap<>();

    public void init() throws ServletException {
        String configFile = getInitParameter("configFile");
        Properties prop = new Properties();
        String configFilePath = getServletContext().getRealPath(configFile);

        try (FileReader fis = new FileReader(configFilePath)) {
            prop.load(fis);
        } catch (IOException e) {
            throw new ServletException(e);
        }

        Iterator keyIter = prop.keySet().iterator();
        while (keyIter.hasNext()) {
            String command = (String) keyIter.next();
            String handlerClassName = prop.getProperty(command);
            try {
                Class<?> handlerClass = Class.forName(handlerClassName);
                CommandHandler handlerInstance = (CommandHandler) handlerClass.newInstance();
                commandHandlerMap.put(command, handlerInstance);
            } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {
                throw new ServletException(e);
            }
        }
    }

    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        process(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        process(request, response);
    }

    private void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String command = request.getParameter("cmd");
        CommandHandler handler = commandHandlerMap.get(command);
        if (handler == null) {
            handler = new NullHandler();
        }
        String viewPage = null;
        try {
            viewPage = handler.process(request, response);
        } catch (Throwable e) {
            throw new ServletException(e);
        }
        if (viewPage != null) {
            RequestDispatcher dispatcher = request.getRequestDispatcher(viewPage);
            dispatcher.forward(request, response);
        }
    }
}
```

- configFile 초기화 파라미터를 설정 파일 경로로 사용하므로, web.xml 파일에 <init-param> 태그를 통해 설정 파일 경로를 지정함

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>ControllerUsingFile</servlet-name>
        <servlet-class>mvc.controller.ControllerUsingFile</servlet-class>
        <init-param>
            <param-name>configFile</param-name>
            <param-value>/WEB-INF/commandHandler.properties</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>ControllerUsingFile</servlet-name>
        <url-pattern>/controllerUsingFile</url-pattern>
    </servlet-mapping>
</web-app>
```

- Properties는 프로퍼티를 관리할 때 사용하는 클래스이고, 프로퍼티는 (이름, 값)으로 구성됨
- Properties 클래스 기본 사용법:

```java
Properties prop=new Properties();
        prop.setProperty("name1","value1");  // 이름이 name 1이고 값이 value1인 프로퍼티 설정
        String name1=prop.getProperty("name1"); // name1 프로퍼티 값 구함
```

- Propertis 클래스는 프로퍼티 목록을 파일에서 읽어올 수 있고, 프로퍼티 정보를 갖고 있는 파일을 프로퍼티 파일이라고함
- 프로퍼티 파일의 형식: #으로 시작하는 줄은 주석으로 처리하고, 한 줄에 한 개의 프로퍼티 이름과 값을 설정, 프로퍼티 이름과 값은 등호 기호(=)를 사용해서 구분

```text
# 주석
프로퍼티이름1=프로퍼티값 
프로퍼티이름2=프로퍼티값2
```

## 요청 URI를 명령어로 사용하기

- 명령어 기반의 파라미터의 단점: 컨트롤러의 URL이 사용자에게 노출됨
    - 사용자가 아무 명령어나 변경해서 컨트롤러에게 전송할 수 있음

```text
http://localhost:8080/chap18/controllerUsingFile?cmd=hello
```

- 이러한 공격을 방지하려면, 요청 URI 자체를 명령어로 사용하는 것이 좋음
- 예시 - 컨트롤러 서블릿의 process() 메서드에서 request.getParameter() 대신 request.getRequestURL() 메서드 사용

```java
String command=request.getRequestURI();
        if(command.indexOf(request.getContextPath())==0){
        command=command.substring(request.getContextPath().length());
        }
```

- 예시 - ControllerUsingFile 컨트롤러 서블릿을 요청 URL를 명령어로 사용하도록 변경

```java
import java.io.FileReader;
import java.io.IOException;
import java.util.HashMap;
import java.util.Iterator;
import
        java.util.Map;
import java.util.Properties;
import javax.servlet.RequestDispatcher;
import
        javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import mvc.command.CommandHandler;
import mvc.command.NullHandler;

public class ControllerUsingURI extends HttpServlet { // <커맨드, 핸들러인스턴스> 매핑 정보 저장 private Map<String, CommandHandler>
    commandHandlerMap =new HashMap<>();

    public void init() throws ServletException {
        String configFile = getInitParameter("configFile");
        Properties prop = new Properties();
        String configFilePath = getServletContext().getRealPath(configFile);
        try (FileReader fis = new FileReader(configFilePath)) {
            prop.load(fis);
        } catch (IOException e) {
            throw new ServletException(e);
        }
        Iterator keyIter = prop.keySet().iterator();
        while (keyIter.hasNext()) {
            String command = (String) keyIter.next();
            String handlerClassName = prop.getProperty(command);
            try {
                Class<?> handlerClass = Class.forName(handlerClassName);
                CommandHandler handlerInstance = (CommandHandler) handlerClass.newInstance();
                commandHandlerMap.put(command, handlerInstance);
            } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {
                throw new ServletException(e);
            }
        }
    }

    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        process(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        process(request, response);
    }

    private void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String command = request.getRequestURI();
        if (command.indexOf(request.getContextPath()) == 0) {
            command = command.substring(request.getContextPath().length());
        }
        CommandHandler handler = commandHandlerMap.get(command);
        if (handler == null) {
            handler = new NullHandler();
        }
        String viewPage = null;
        try {
            viewPage = handler.process(request, response);
        } catch (Throwable e) {
            throw new ServletException(e);
        }
        if (viewPage != null) {
            RequestDispatcher dispatcher = request.getRequestDispatcher(viewPage);
            dispatcher.forward(request, response);
        }
    }
}
```

- 특정 확장자(예: .do)를 가진 요청을 ControllerUsingFileURI 컨트롤러 서블릿이 처리하도록, web.xml 파일에 <servlet-maaping> 정보를 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>ControllerUsingURI</servlet-name>
        <servlet-class>mvc.controller.ControllerUsingURI</servlet-class>
        <init-param>
            <param-name>configFile</param-name>
            <param-value>/WEB-INF/commandHandlerURI.properties</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>ControllerUsingURI</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```

- servlet-mapping 태그를 web.xml 파일에 추가하면 *.do로 오는 요청은 ControllerUsingURI 컨트롤러 서블릿을 전달됨
- 요청 URI를 명령어로 사용하는 ControllerUsingURI에 알맞은 설정 파일을 작성

```text
/hello.do=mvc.hello.HelloHandler
```
