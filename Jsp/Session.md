# 세션(session)이란?
- 세션은 웹 컨테이너에 클라이언트의 상태 정보를 보관할 때 사용함 
- 쿠키(웹 브라우저에 정보 보관) vs 세션(서버에 정보 보관)
- 웹 컨테이너는 기본적으로 한 웹 브라우저마다 한 세션을 생성함

![img](https://user-images.githubusercontent.com/89081374/212525516-964941dc-47b2-4c6f-a904-c10c7138af4b.png)

## 쿠키(cookie) vs 세션(session)
- 세션이 쿠키보다 보안적으로 우위
  - 쿠키의 이름이나 데이터는 네트워크를 통해 전달 -> HTTP 프로토콜을 사용하는 경우 중간에 탈취 가능
  - 세션의 값은 오직 서버에만 저장
- 웹 브라우저가 쿠키를 지원하지 않거나 강제적으로 쿠키를 막은 경우 쿠키는 사용할 수 없게 되지만, 세션은 쿠키 설정 여부와 상관없이 사용할 수 있음
  - 서블릿/JSP는 쿠키를 사용할 수 없는 경우, URL 재작성 방식을 사용해서 세션 ID를 웹 브라우저와 웹 서버가 공유 가능
- 세션은 여러 서버에서 공유할 수 없지만, (www.daum,net과 mail.daum.net의 세션은 다름) 쿠키는 여러 도메인 주소에 공유할 수 있음
  - 이러한 이유로 포털 사이트는 쿠키를 이용해서 로그인 정보를 저장함

## session 생성하기
- JSP에서 세션 생성하는 방법: page 디렉티브의 session 속성을 true로 지정
```html
<%@ page session="true" %>
```
- page 디렉티브 session 속성의 기본값은 true이므로 session 속성을 false로 지정하지만 않으면 세션이 생성 
- 세션을 사용하는 서버 프로그램은 웹 브라우저가 처음 접속할 때 세션을 생성, 이후 기존에 생성된 세션 사용

## session 기본 객체
- 세션이 생성되면, session 기본 객체를 통해 세션 사용 가능 
- session 기본 객체는 속성을 제공하므로 setAttribute(), getAttribute() 등의 메서드를 사용하여 속성값을 저장하거나 읽어올 수 있음 
- session 기본 객체가 제공하는 세션 정보 관련 메서드

|메서드| 리턴 타입|	설명|
|-----|-------|----|
|getId() | String | 세션의 고유 ID를 구함 (세션 ID)|
|getCreationTime()|	long |세션이 생성된 시간을 구함, 시간은 1970년 1월 1일 이후 흘러간 시간을 의미, 단위는 1/1000초|
|getLastAccessedTime()|long| 웹 브라우저가 가장 마지막에 세션에 접근한 시간을 구함, 시간은 1970년 1월 1일 이후 흘러간 시간을 의미, 단위는 1/1000초|
- 세션 ID: 웹 브라우저마다 생성되는 별도의 세션을 구분하기 위한 세션의 고유 ID
- JSESSIONID: 웹 서버와 웹 브라우저가 세션 ID를 공유하기 위해 사용하는 쿠키 
- 웹 서버는 세션 ID를 이용해서 웹 브라우저를 위한 세션을 찾기 때문에 세션 ID를 공유할 수 있는 방법이 필요함

## 기본 객체의 속성 사용
- 로그인한 회원 정보 등 웹 브라우저와 일대일로 관련된 값을 저장할 때는 쿠키 대신 세션을 사용
  - request 기본 객체: 하나의 요청을 처리하는 데 사용되는 JSP 페이지 사이에서 공유
  - session 기본 객체: 웹 브라우저의 여러 요청을 처리하는 JSP 페이지 사이에서 공유
- 세션에 값을 저장할 때는 속성을 사용 
- setAttribute() 메서드: 속성에 값 저장 
- getAttribute() 메서드: 속성값 읽기

- 예시 - 세션 값 저장 및 가져오기
```java
session.setAttribute("NAME","Hi");
String name = (String)session.getAttribute("NAME");
```
## 세션 종료
- 세션을 유지할 필요가 없으면 session.invalidate() 메서드를 사용해서 세션 종료
- 세션을 종료하면 현재 사용 중인 session 기본 객체와 session 기본 객체에 저장했던 속성 목록도 함께 삭제함


## 세션 유효 시간
- 세션은 최근 접근 시간을 갖고, 확인이 가능함
```java
session.getLastAccessedTime()
```
- session 기본 객체를 사용할 때마다 세션의 최근 접근 시간을 갱신함
- 세션은 마지막 접근 이후 일정 시간 이내에 다시 세션에 접근하지 않는 경우 자동으로 세션을 종료하는 기능이 존재함 (타임 아웃 기능)

![img](https://user-images.githubusercontent.com/89081374/212525940-552fa979-c807-4b88-9d75-a36235875254.png)

- 세션 유효 시간을 설정하는 두 가지 방법
  - WEB-INFO\web.xml 파일에 <session-config> 태그를 사용하여 세션 유효 시간 지정 (분 단위)
     - 주의! <session-timeout> 값을 0이나 음수로 설정하면 유효 시간을 갖지 않아, session.invalidate() 메서드를 호출하기 전까지 세션 객체가 계속 서버에 유지되어 메모리 부족 현상이 발생할 수 있음
    ```html
    <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
            <session-config>
                <session-timeout>50</session-timeout>
            </session-config>
        </web-app>
    ```
  - session 기본 객체가 제공하는 setMaxInactiveInterval() 메서드 사용 (초 단위)
```html
<%
session.setMaxInactiveInterval(60*60);
%>
```

## request.getSession을 이용한 세션 생성
- request 기본 객체의 getSession() 메서드를 사용하여 HttpSession을 생성 가능
- 예시 - page 디렉티브의 session 속성값을 false로 지정하고, request.getSession()을 이용해 세션 구하기
```html
<%@ page session="false" %>
<%
HttpSession httpSession = request.getSession();
List list = (List)httpSesssion.getAttribute("list");
list.add(productId);
%>
```
- request.getSession() 메서드는 session이 존재하면 해당 session을 리턴하고, 존재하지 않으면 새롭게 session을 생성해서 리턴
-  getSession() 메서드에 false 인자를 전달하여 실행하면, 세션이 존재하는 경우에는 session 객체를 리턴하고, 세션이 존재하지 않으면 null을 리턴

## 세션에 여러 속성을 사용해서 연관 정보 저장하기
- 세션에 여러 속성을 사용해서 연관 정보들을 저장할 때 클래스를 사용하면 문제점을 줄일 수 있음
- 예시 - 사용자 정보를 저장하는 MemberInfo 클래스 사용

```java
public class MemberInfo{
private String id;
private String name;
private String email;
private boolean male;
private int age;

    //get 메서드
}
```
- 연관된 정보를 클래스로 묶어서 저장하면 여러 정보를 한 개의 속성을 이용해 저장 가능
```html
<%
MemberInfo memberInfo = new MemberInfo(id, name);
session.setAttribute("memberInfo", memberInfo);
%>
```
-  연관된 정보를 한 객체에 담아 저장하므로, 세션에 저장한 객체를 가져와 필요한 값을 읽음

```
<%
MemberInfo memberInfo = (MbemberInfo)session.getAttribute("memberInfo");
%>
...
<%= member.getEmail().toLowerCase()%>
```
- 이때, 이메일 주소를 저장할 필요가 없어 Member클래스에 getEmail() 메서드를 삭제하면, NullPointerException이 아닌 컴파일 에러가 발생
-> Exception 추적보다 컴파일 에러를 처리하는 것이 상대적으로 쉬우므로 개발자가 문제를 해결하기 쉬워짐

# 서블릿 컨텍스트와 세션
- 같은 서버에서 서로 다른 경로는 서로 다른 웹 어플리케이션이므로, 각각의 JSESSIOID를 사용함
  - http://localhost:8080/ch10/viewCookies.jsp, http://localhost:8080/ch10_2/viewCookies.jsp는 다른 웹 어플리케이션
      - 즉, 서로 다른 웹 어플리케이션은 같은 웹 브라우저라도 세션을 공유하지 않음
- 세션 ID를 보관할 때 사용할 JSESSIONID 쿠키의 경로로 웹 어플리케이션의 컨텍스트 경로를 사용함
  - 웹 어플리케이션의 컨텍스트 경로가 /ch10이면, 세션을 위한 JSESSIONID 쿠키의 경로도 ch10이고,
    경로가 /ch10인 쿠키는 URL이 /ch10 경로로 시작하는 경우에만 전송되므로,
    /ch10 웹 어플리케이션에서 생성한 JSESSIONID 쿠키는 /ch10 웹 어플리케이션에서만 사용함
  
* 컨텍스트 경로(Context Path): 프로젝트 명을 의미하며 url의 호스트, 포트명 다음에 나온다.