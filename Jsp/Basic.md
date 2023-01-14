# URL과 웹페이지
## URL 
- Uniform Resource Locator(통합 자원 위치)의 약자로 일종의 주소와 같은 역할
- https://www.tistory.com/ , https://www.google.com/ 과 같이 웹브라우저 주소줄에 표시되는 것
- 웹페이지: 웹 브라우저에 URL에 해당하는 내용이 출력되는 것
- 홈페이지, 웹 사이트: 웹 페이지의 묶음

### URL의 주요 구성 요소

|구성 요소|설명|  
|:----|:------|
|프로토콜	| 웹 브라우저가 서버와 내용을 주고받을 때 사용할 규칙 이름|
|서버 이름| 웹 페이지를 요청할 서버의 이름 지정, 도메인 이름(예:tistory.com) 또는 180.70.134.239와 같은 IP 주소 입력|
|경로 | 웹 페이지의 상세 주소 | 
|쿼리 문자열| 추가로 서버에 보내는 데이터,같은 경로에서 입력한 값에 따라 다른 결과를 보여줄 때 사용 ex) 검색 결과를 보여주는 페이지|
![jpg](https://user-images.githubusercontent.com/89081374/212466054-27e3a81a-493c-48b8-845b-e9a3cb25e403.png)

### URI(identifier) vs URN(name) vs URL(location)
```text
URI(Uniform Resource Identifier) => https://www.google.com/folder/page.html
URL(Uniform Resource Locator) => https://www.google.com/
URN(Uniform Resource Name) => /folder/page.html
```

# 웹 브라우저와 웹 서버
웹 브라우저에 URL을 입력하면, 웹 서버 프로그램이 웹 브라우저에 웹 페이지를 제공함
![jpg](https://user-images.githubusercontent.com/89081374/212466138-5b1d1961-b7d2-46bf-a91c-c83501b87719.jpeg)
- 웹 브라우저가 웹 서버에 웹 페이지를 달라고 하는 것을 요청(request), 요청한 웹 페이지를 웹 브라우저에 제공하는 것을 응답(response)이라고 표현
- IP 주소: 웹 브라우저와 웹 서버를 연결하기 위해 필요한 웹 서버가 실행 중인 컴퓨터의 주소 ex)180.70.134.239
- 도메인 이름: IP 주소는 숫자로 구성되어 외우기 어렵기 때문에 사용되는 문자(0~9, a~z, 하이픈)로 이루어진 주소 이름
- DSN(Domain Name Server): 도메인 이름을 IP 주소로 변환하는 서버
- 포트(port): 한 개의 컴퓨터의 웹 서버, 스트리밍 서버, 채팅 서버 등 다양한 서버 프로그램이 존재할 수 있으므로, 클라이언트와 서버 프로그램을 연결할 때 다른 서버 프로그램과 구분할 수 있도록 포트를 사용

# HTML과 HTTP
- HTML(HyperText MarkUp Language):  웹 페이지의 모습을 기술하기 위한 규약
- 렌더링(Rendering): 웹서버에서 전송한 HTML 문서를 받은 웹 브라우저에서 이를 분석하여, HTML 표준 규칙에 따라 알맞은 화면을 생성하는 과정
- HTTP(HyperText Transfer Protocol): 클라이언트와 웹 서버가 HTML, 이미지, 동영상, XML 문서 등 다양한 데이터를 주고받을 때 사용하는 규칙
  - 요청 규칙: 클라이언트가 웹 서버에 데이터를 요청할 때 사용할 데이터 구성 규칙
  - 응답 규칙: 웹 서버가 클라이언트에 데이터를 전송할 때 사용할 데이터 구성 규칙
![img](https://user-images.githubusercontent.com/89081374/212466219-3dc13863-0af1-4c51-b600-50a7e2d2b6aa.png)
<HTTP 요청 데이터와 응답 데이터>

|구성 요소|요청 데이터|응답 데이터|예시|
|-------|--------|--------|---|
|요청/응답 줄 | GET, POST 같은 HTTP 요청 방식과 요청하는 자원의 경로 지정 | 요청에 대한 응답 코드 전송 |ex) 200	GET / HTTP/1.1 (요청), HTTP/1.1 200 OK (응답)|
|헤더 | 서버가 응답을 생성하는데 참조할 수 있는 정보 전송 ex) 브라우저의 종류, 언어 정보 등	 |  응답에 대한 정보 전송 ex) 몸체의 데이터, 길이 등의 정보	 | Host: www.daum.net Date: Wed, 22 Apr 2015 ~ |
|몸체 | 정보를 전송해야 할 때 사용 ex) 파일 업로드시 몸체에 파일을 담아 웹서버에 전송	 | 웹 브라우저가 요청한 자원의 내용을 담음 ex) HTML 문서, 이미지 파일, 데이터 등  | <!DOCTYPE html> ~~~ | 
- 헤더 영역은 요청/응답 줄 다음에 위치하며, "헤더이름:헤더 값"으로 구성
- 헤더가 끝난 후 빈 줄이 오고 그 다음 몸체 내용이 온다



# 정적 자원과 동적 자원
- 정적(static) 자원 또는 페이지: URL로 요청했을 때 고정된 결과가 출력되는 자원 ex) HTML 파일 등

- 동적(dynamic) 자원 또는 페이지: URL로 요청했을 때 시간 등 특정 조건에 따라 응답 데이터가 달라지는 자원
ex) PHP, JSP 등

# 웹 프로그래밍과 JSP
- 웹 프로그래밍: 웹 서버가 웹 브라우저에 응답으로 전송할 데이터를 생성해주는 프로그램을 작성하는 것
- 웹 서버의 종류에 따라 웹 프로그래밍을 할 때 사용하는 기술이 달라짐 ex) Apache - php, Window IIS - ASP.net
- JSP(Java Server Page): 동적 페이지를 작성하는데 사용되는 자바의 표준 기술
- WAS(Web Application Server)
  - JSP를 이용해서 만든 프로그램을 실행하기위해 필요
  - 웹을 위한 연결, 프로그래밍 언어, 데이터베이스 연동 등 어플리케이션을 구현하는 데 필요한 기능을 제공
  - 웹 브라우저로부터 요청이 오면 알맞은 프로그램을 찾아 실행하고, 프로그램의 실행 결과를 응답으로 전송하는 기능 제공