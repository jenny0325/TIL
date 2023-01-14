# 쿠키란?
- 쿠키(cookie): 웹 브라우저가 보관하는 데이터
- 웹 브라우저는 웹 서버에 요청을 보낼 때 쿠키를 함께 전송, 웹 서버는 쿠키를 사용해 필요한 데이터를 읽을 수 있음
- 쿠키는 웹 서버와 웹 브라우저 양쪽에서 생성 가능
- JSP에서 생성하는 쿠키는 웹 서버에서 생성하는 쿠키

## 쿠키 동작 방식
![img](https://user-images.githubusercontent.com/89081374/212471128-6f11a26e-fa57-42ee-8e73-facea9ed086f.png)
- 쿠키 생성: JSP 프로그래밍에서 쿠키는 웹 서버 측에서 생성, 생성한 쿠키를 응답 데이터 헤더에 저장해서 웹 브라우저에 전송
- 쿠키 저장: 웹 브라우저는 응답 데이터에 포함된 쿠키를 쿠키 저장소에 보관, 쿠키의 종류에 따라 메모리나 파일에 저장
- 쿠키 전송: 웹 브라우저는 저장한 쿠키를 요청이 있을 때마다 웹 서버에 전송, 웹 서버는 해당 쿠키를 사용해서 필요한 작업 수행
- 웹 브라우저에 쿠키가 생성되면, 웹 브라우저는 쿠키가 삭제되기 전까지 웹 서버에 쿠키를 전송함
  - 웹 어플리케이션을 사용하는 동안 지속적으로 유지해야 하는 정보를 쿠키를 사용해서 저장하기



## 쿠키의 구성
- 이름: 각각의 쿠키를 구별하는 데 사용 (웹 브라우저는 여러 개의 쿠키를 가질 수 있음)
  - 쿠키 이름은 콤마, 세미콜론, 공백, 등호기호('=')를 제외한 출력 가능한 아스키 문자로 구성
- 값: 쿠키의 이름과 관련된 값
  - 서버는 값을 사용해서 원하는 작업 수행
  - 쿠키 값은 콤마, 세미콜론, 공백 문자를 제외한 나머지 출력 가능한 아스키 문자를 사용
  - 값으로 사용 가능한 문자가 한정되므로 쿠키 값을 생성할 때는 알맞은 방식으로 인코딩함
  - ex) 자바는 URL 인코딩을 사용해 쿠키 값으로 사용할 문자열을 변환 가능
- 유효시간: 쿠키의 유지 시간
  - 별도 유효 시간을 지정하지 않으면 웹 브라우저를 종료할 때 쿠키를 함께 삭제함
- 도메인: 쿠키를 전송할 도메인 
- 경로: 쿠키를 전송할 요청 경로



## 쿠키 생성하기
- JSP는 Cookie 클래스를 사용하여 쿠키를 생성함
```html
<%
Cookie cookie = new Cookie("cookieName","cookieValue");
response.addCookie(cookie);
%>
```
- Cookie 클래스 생성자의 첫 번째 인자는 쿠키 이름, 두 번째 인자는 쿠키 값을 의미
- response 기본 객체에 addCookie() 메서드를 사용하여 쿠키를 추가
  - reponse 기본 객체는 웹 브라우저에 쿠키 정보를 전송
- 예시 - 쿠키 생성
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>

<%@ page import = "java.net.URLEncoder"%>

<%
Cookie cookie = new Cookie("name", URLEncoder.encode("Hi", "utf-8"));
response.addCookie(cookie);
%>

<html>
    <head><title>쿠키생성</title></head>
    <body>

        <%= cookie.getName() %> 쿠키의 값 ="<%=cookie.getValue() %>"
        
    </body>
</html>
```
```text
결과
name 쿠키의 값 = "Hi"
```
- Cookie 클래스가 제공하는 메서드

|메서드|리턴 타입|설명|
|----|-------|---|
|getName()|String|쿠키 이름 가져오기|
|getValue()|String|쿠키 값 가져오기|
|setValue(String value)|void|쿠키 값 지정|
|setDomain(String pattern)|void|쿠키가 전송될 서버의 도메인 지정|
|getDomain()|String|쿠키의 도메인 가져오기|
|setPath(String uri)|void|쿠키를 전송할 경로 지정|
|getPath()|String|쿠키 전송 경로 가져오기|
|setMaxAge(int expiry)|void|쿠키의 유효시간을 초 단위로 지정, 음수를 입력할 경우 웹 브라우저를 닫을 때 쿠키가 함께 삭제|
|getMaxAge()|int|쿠키 유효시간 가져오기|

## 쿠키 값 읽어오기
- 웹 브라우저는 요청 헤더에 쿠키를 저장해서 보내고, JSP는 request 기본 객체의 getCookies() 메서드로 쿠키 값을 읽음
```java
Cookie[] cookies = request.getCookies();
```
- 쿠키가 존재하지 않으면 null을 리턴

## 쿠키 값 변경 및 쿠키 삭제
- 쿠키 값 변경 방법: 같은 이름을 가진 쿠키 객체를 생성하여 응답 데이터에 추가
```java
Cookie cookie = new Cookie("name",URLEncoder.encode("새로운값","euc-kr"));
response.addCookie(cookie);
```
- 쿠키 존재를 확인하는 코드
```java
Cookie[] cookies = request.getCookies();
if(cookies != null && cokies.length > 0{
  for(int i = 0; i < cookies.length; i++){
    if(cookies[i].getName().equals("name")){
      Cookie cookie = new Cookie("name", URLEncoder.encode("변경할값", " utf-8"));
      response.addCookie(cookie);
    }
  }
}
```
- 쿠키 삭제 방법: 유효시간을 0으로 지정한 후 응답 헤더에 추가
```java
Cookie cookie = new Cookie(name, value);
cookie.setMaxAge(0);
response.addCookie(cookie);
```

## 쿠키 도메인
- setDomain() 메서드: 생성한 쿠키를 전송할 수 있는 도메인을 지정
  - .somehost.com: 점으로 시작하는 경우 관련 도메인에 모두 쿠키 전송 ex) mail.somehost.com, www.somehost.com, javacan.somehost.com
  - www.somehost.com:특정 도메인에 대해서만 쿠키 전송
- 주의! 도메인을 지정할 때 setDomain()의 값으로 현재 서버의 도메인 및 상위 도메인만 전달 가능
  - 이외의 도메인을 전달하면, 웹 브라우저는 쿠키를 저장하지 않음
  - ex) JSP페이지가 실행되는 서버의 주소가 mail.somehost.com 일 경우 setDomain() 메서드에 "mail.somehost.com", ".somehost.com"만 전달 가능. "www.somehost.com"은 불가능
- 예시
```java
Cookie cookie = new Cookie("name", "Ted");
cookie.setDomain(".somehost.com");
response.addCookie(cookie);
```

## 쿠키의 경로
- Cookie 클래스의 setPath() 메서드로 쿠키를 공유할 기준 경로를 지정 
- 경로: URL에서 도메인 이후의 부분 
- ex) http://localhost:8080/chap09/path2/viewCookies.jsp 에서 /chap09/path2/viewCookies.jsp가 경로
- 쿠키는 디렉터리 수준 경로를 사용하여 위에서는 "/", "/chap09", "/chap09/path2" 등을 쿠키 경로로 사용 가능 
- setPath() 메서드를 사용하여 쿠키의 경로를 지정하면, 웹 브라우저는 지정한 경로 또는 하위 경로에 대해서만 쿠키를 전송함

## 쿠키 유효시간
- 쿠키에 유효시간을 정해 놓으면 그 유효시간 동안 쿠키가 존재하며, 웹 브라우저를 종료해도 유효시간이 지나지 않았으면 쿠키를 삭제하지 않음
- 유효시간을 지정하지 않으면 웹 브라우저를 종료할 때 쿠키를 함께 삭제함
- JSP에서는 setMaxAge() 메서드를 사용하여 초 단위로 유효 시간을 지정함
```java
Cookie cookie = new Cookie(name, value);
cookie.setMaxAge(60*60); //60초 * 60 = 1시간
response.addCookie(cookie);
```
## 쿠키와 헤더
- response.addCookie() 메서드로 쿠키를 추가하면, Set-Cookie 헤더를 통해 전달됨
```text
쿠키이름=쿠기값; Domain=도메인값; Path=경로값; Expires=GMT형식의만료일시
* GMT: 그리니티 평균시 (https://ko.wikipedia.org/wiki/그리니치_평균시)
```
- 한 개의 Set-Cookie 헤더는 한 개의 쿠키 값을 전달함 
- 쿠키는 응답 헤더를 사용해서 웹 브라우저에 전달하므로, 출력 버퍼를 플러시하기 전에 추가해야함

# 쿠키 처리를 위한 유틸리티 클래스
- 예시 - 쿠키를 편리하게 사용할 수 있도록 도와주는 보조 유틸리티 클래스
```java
package util;

import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServletRequest;

import java.io.IOException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.util.Map;

public class Cookies {

    private Map<String, Cookie> cookieMap = new java.util.HashMap<String, Cookie>();
        
        public Cookies(HttpServletRequest request) {
            Cookie[] cookies = request.getCookies();
            if(cookies != null){
                for (int i = 0; i < cookies.length; i++) {
                    cookieMap.put(cookies[i].getName(), cookies[i]);
                }
            }
        }
        
        public Cookie getCookie(String name) {
            return cookieMap.get(name);
        }
        
        public String getValue(String name) throws IOException {
            Cookie cookie = cookieMap.get(name);
            if(cookie == null){
                return null;
            }
            return URLDecoder.decode(cookie.getValue(), "euc-kr");
        }
        
        public boolean exists(String name) {
            return cookieMap.get(name) != null;
        }
        
        public static Cookie createCookie(String name, String value) throws IOException {
            return new Cookie(name, URLEncoder.encode(value, "utf-8"));
        }
        
        public static Cookie createCookie(String name, String value, String path, int maxAge) throws IOException {
            Cookie cookie = new Cookie(name, URLEncoder.encode(value, "utf-8"));
            cookie.setPath(path);
            cookie.setMaxAge(maxAge);
            return cookie;
        }
        
        public static Cookie createCookie(String name, String value, String domain, String path, int maxAge) throws IOException {
            Cookie cookie = new Cookie(name, URLEncoder.encode(value, "utf-8"));
            cookie.setDomain(domain);
            cookie.setPath(path);
            cookie.setMaxAge(maxAge);
            return cookie;
        }
}

```
