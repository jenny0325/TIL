# **1\. Connection Pool이란?**

Connection Pool에서 Connection이란 WAS(Web Application Server)와 DB 사이의 연결을 말한다.

``` java
Connection conn = null;
PreparedStatement  pstmt = null;
ResultSet rs = null;

try {
    sql = "SELECT * FROM T_BOARD"

    // 1. 드라이버 연결 DB 커넥션 객체를 얻음
    connection = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);

    // 2. 쿼리 수행을 위한 PreparedStatement 객체 생성
    pstmt = conn.createStatement();

    // 3. executeQuery: 쿼리 실행 후
    // ResultSet: DB 레코드 ResultSet에 객체에 담김
    rs = pstmt.executeQuery(sql);
    } catch (Exception e) {
    } finally {
        conn.close();
        pstmt.close();
        rs.close();
    }
}
```

위와 같이 자바에서 DB에 직접 연결해서 처리하는 경우 JDBC Driver를 로드하고 Connection 객체를 받아와야 한다. 그러면 매번 사용자가 요청을 할 때마다 드라이버를 로드하고 Connection 객체를 생성하여 연결하고 종료하기 때문에 매우 비효율적이다.

또한 Connection을 생성하는 작업은 굉장히 많은 비용이 드는 작업이다.

아래의 내용은 MySQL 8.0 기준으로 INSERT문을 수행하는 과정을 나타낸다. 괄호 안의 숫자는 각 과정을 수행하는 데 필요한 비용의 비율을 의미한다.

-   _MySQL 8.0 Documentation:[https://dev.mysql.com/doc/refman/8.0/en/insert-optimization.html](https://dev.mysql.com/doc/refman/8.0/en/insert-optimization.html)_
-   Connecting: (3)
-   Sending query to server: (2)
-   Parsing query: (2)
-   Inserting row: (1)
-   Inserting index: (1)
-   Closing: (1)

가장 많은 비용이 드는 작업은 무엇일까?

보다 시피 가장 비용이 많이 드는 작업은 바로 서버가 DB 접속하기 위해서 Connection을 생성하는 작업이다.

즉, Connection을 반복적으로 생성하는 것은 그만큼 큰 비용이 드는 작업임을 알 수 있다.

이런 문제를 해결하기 위해서 Connection Pool을 사용한다.

>**Connection Pool이란**   
> 웹 컨테이너(WAS)가 실행되면서 일정량의 **Connection 객체를 미리 만들어서 pool에 저장**했다가, 클라이언트 요청이 오면 **Connection 객체를 빌려**주고 해당 객체의 임무가 완료되면 **다시 Connection 객체를 반납 받아서 pool에 저장**하는 프로그래밍 기법이다.

![img](https://user-images.githubusercontent.com/89081374/218775976-6d48c9b9-b815-4c72-bcf8-c3e8a19e3b6f.png)

### **<Connection Pool의 장점>**

-   DB 접속 설정 객체를 미리 만들어 연결하여 메모리 상에 등록해 놓기 때문에 불필요한 작업(Connection 생성, 삭제)이 사라지므로 클라이언트가 빠르게 DB에 접속이 가능하다.
-   DB Connection 수를 제한할 수 있어서 과도한 접속으로 인한 서버 자원 고갈 방지가 가능하다.
-   DB 접속 모듈을 공통화하여 DB 서버의 환경이 바뀔 경우 쉬운 유지 보수가 가능하다.
-   연결이 끝난 Connection을 재사용함으로써 새로 객체를 만드는 비용을 줄일 수 있다.

이러한 Connection Pool은 어떻게 동작할까?

# **2\. Connection Pool 동작방식**

Spring의 default JDBC Connection Pool인 Hikari CP가 동작하는 방식을 통해서 Connection Pool이 동작하는 원리에 대해 간단히 알아보자.

![img](https://user-images.githubusercontent.com/89081374/218776156-695540cf-d9b5-41df-adb9-9e4c50c1492a.png)

그림에서 볼 수 있듯이 Thread가 Connection을 요청하면 Connection Pool의 각자의 방식에 따라 사용가능한 Connection을 찾아서 반환한다. Hikari CP의 경우, 이전에 사용했던 Connection이 존재하는지 확인하고, 이를 우선적으로 반환하는 특징을 갖고 있다.

![img](https://user-images.githubusercontent.com/89081374/218776259-84565cda-7e9c-4e2b-86bf-80067ae417a8.png)

만약에 가능한 Connection이 존재하지 않으면 HandOffQueue를 Polling 하면서 다른 Thread가 Connection을 반납하기를 기다린다. 이를 지정한 TimeOut 시간까지 대기하다가 시간이 만료되면 Exception을 던진다.

![img](https://user-images.githubusercontent.com/89081374/218776347-d9a4814f-1326-4745-bf41-2403e2627014.png)


최종적으로 사용한 Connection을 반납하면 Connection Pool이 사용내역을 기록하고, HandOffQueue에 반납된 Connection을 삽입한다. 이를 통해 HandOffQueue를 Polling 하던 Thread는 Connection을 획득하고 작업을 이어나갈 수 있게 된다.

이렇게 Thread들은 트랜잭션을 처리하기 위해서 Connection을 획득하고, 이를 반납함으로써 상대적으로 비싼 작업인 Connection 생성을 줄일 수 있다.

그러나, 위에서 본 것 처럼 Connection Pool의 크기가 작으면 Connection을 획득하기 위해 대기하는 Thread가 많아지고, 이에 따라 성능적인 저하를 발생시킬 수 있다.

이는 Connection Pool의 크기를 늘려주면 쉽게 해결할 수 있다. 그렇다면 Connection Pool에 저장된 Connection 개수가 무제한으로 커지면 성능이 좋아질까?

# **3\. Connection Pool의 크기가 크면 클 수록 좋을까?**

정답은 No 이다.

Connection의 주체는 Thread이므로 Thread와 함께 고려해야 한다.

만약 Thread Pool의 크기보다 Connection Pool의 크기가 크면 어떻게 될까?

Thread Pool에서 트랜잭션을 처리하는 Thread가 사용하는 Connection 외에 남는 Connection은 실질적으로 메모리 공간만 차지하게 된다.

그렇다면 Thread Pool의 크기와 Connection Pool의 크기를 함께 키워주면 어떻게 될까?

우선 Thread 증가로 인해 더 많은 Context Switching이 발생한다. 그리고 Disk 경합 측면에서 성능 한계가 발생한다. 데이터베이스는 하드 디스크 하나 당 하나의 I/O를 처리하므로 블로킹이 발생한다. SSD를 사용하면 일부 병렬 처리가 가능하지만 이 또한 한계가 존재한다. 즉, 특정 시점부터는 성능적인 증가가 Disk 병목으로 인해 미비해진다.

따라서 데이터베이스 입장에서 Connection은 Thread와 어느 정도 일치한다고 볼 수 있다. Connection이 많다는 의미는 데이터베이스 서버가 Thread를 많이 사용한다는 것을 의미하고, 이에 따라 Context Switching으로 인한 오버헤드가 더 많이 발생하기 때문에 Connection Pool을 아무리 늘리더라도 성능적인 한계가 존재한다.

따라서 사용량에 따라 적정량의 Connection 객체를 생성해 두어야 한다.

###**그렇다면 적절한 Connection Pool의 크기는 얼마일까?**

Hikari CP의 공식 문서에 의하면, 
1 connections = ((core_count) * 2) + effective_spindle_count)로 정의하고 있다.

여기서 말하는 core_count는 현재 사용하는 Cloud 서버 환경에서는 논리 CPU 개수와 동일하다.

core_count * 2를 하는 이유는 위에서 설명한 Context Switching 및 Disk I/O 와 관련이 있다. Context Switching으로 인한 오버헤드를 고려하더라도 데이터베이스에서 Disk I/O 혹은, DRAM이 처리하는 속도보다 CPU 속도가 월등히 빠르기 때문에 Thread가 Disk와 같은 작업에서 Blocking 되는 시간에 다른 Thread의 작업을 처리할 수 있는 여유가 생기게 된다. 이러한 여유 정도에 따라 멀티 스레드 작업을 수행할 수 있게 되고, Hikari CP가 제시한 공식에서는 계수를 2로 선정하여 Thread 개수를 지정하였다.

![6](https://user-images.githubusercontent.com/89081374/218776861-4ca4e762-ed5a-4ba1-aa32-e951b83222d1.png)

effective_spindle_count는 하드 디스크와 관련이 있다. 하드 디스크 하나는 spindle 하나를 가진다. 이에 따라 spindle의 수는 기본적으로 DB 서버가 관리할 수 있는 동시 I/O 요청 수를 말한다. 디스크가 16개가 있는 경우 시스템은 동시에 16개의 I/O 요청을 처리할 수 있다. 물론 RAID 구성 방식에 따라서 달라질 수 있다. 해당 공식에서 디스크의 효율을 고려하여 spindle_count를 더해준 것으로 보인다.

최종적으로 CPU의 처리 효율과 디스크 처리 효율을 고려한 결과, ((core_count * 2) + effective_spindle_count) 공식을 통해 Connection의 개수를 추정할 수 있다.

# **4\. 커넥션 풀(DBCP)의 종류**

## **4.1. commons-dbcp**

아파치에서 제공하는 대표적인 커넥션 풀 라이브러리이다.

![9](https://user-images.githubusercontent.com/89081374/218777382-258dd363-a087-49ba-ba5c-e769aff9059f.png)


![7](https://user-images.githubusercontent.com/89081374/218776878-dca7523a-7012-4d93-94bf-3d8f4c8ecde1.png)

위의 예시에서는 8개의 커넥션을 최대로 활용할 수 있는 상태이며, 4개는 사용 중이고 4개는 대기 중인 상태이다.

-   maxActive ≥ initialSize
    -   최대 커넥션 개수는 초기에 생성할 커넥션 개수와 같거나 크게 설정해야 한다.
-   maxActive = maxIdle
    -   maxActive 값과 maxIdle 값은 같은 것이 바람직하다. 만약 둘의 값이 아래와 같다고 가정해 보자.
    -   maxActive = 10, maxIdle = 5
        -   항상 커넥션을 동시에 5개는 사용하고 있는 상황에서 1개의 커넥션이 추가로 요청된다면 maxActive = 10이므로 1개의 추가 커넥션을 데이터베이스에 연결한 후 pool은 비즈니스 로직으로 커넥션을 전달한다.
        -   이후 비즈니스 로직이 커넥션을 사용한 후 pool에 반납할 경우, maxIdle = 5에 영향을 받아 커넥션을 실제로 닫아버리므로 일부 커넥션을 매번 생성했다 닫는 비용이 발생할 수 있다.

커넥션 개수와 관련된 가장 중요한 성능 요소는 일반적으로 커넥션의 최대 개수이다. 4개 항목의 설정 값 차이는 성능을 좌우하는 중요 변수는 아니다.

maxActive 값은 DBMS의 설정과 애플리케이션 서버의 개수, Apache, Tomcat에서 동시에 처리할 수 있는 사용자 수 등을 고려해서 설정해야 한다. DBMS가 수용할 수 있는 커넥션 개수를 확인한 후에 애플리케이션 서버 인스턴스 1개가 사용하기에 적절한 개수를 설정한다. 사용자가 몰려서 커넥션을 많이 사용할 때는 maxActive 값이 충분히 크지 않다면 병목 지점이 될 수 있다. 반대로 사용자가 적어서 사용 중인 커넥션이 많지 않은 시스템에서는 maxActive 값을 지나치게 작게 설정하지 않는 한 성능에 큰 영향이 없다.

Commons DBCP에서는 DBMS에 로그인을 시도하고 있는 커넥션도 사용 중인 것으로 간주한다. 만약 DBMS에 로그인을 시도하고 있는 상태에서 무한으로 대기하고 있다면, 애플리케이션에서 모든 커넥션이 사용 중인 상태가 돼 새로운 요청을 처리하지 못할 수도 있다. 이런 경우 장애 확산을 최소화하려면 Microsoft SQL Server의 JDBC 드라이버에서 설정하는 loginTimeOut 속성같은 JDBC 드라이버별 타임아웃 속성을 설정하는 것이 좋다.

#### **4.2. tomcat-jdbc-pool**

-   tomcat에 내장되어 사용되고 있다.
-   Apache Commons DBCP 라이브러리를 바탕으로 만들어져 있다.
-   Spring boot 2.0.0 하위 버전에서 사용하는 기본 DBCP이다.

#### **4.3. HikariCP**

-   스프링 부트 2.0부터 default JDBC connection pool이다.
-   zero-overhead의 특징을 갖는다.
    -   overhead: 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간 및 메모리

Springboot 환경에서는 application.properties에서 간단하게 HikariCP의 설정을 할 수 있다.

![8](https://user-images.githubusercontent.com/89081374/218776889-28b17b75-8b9f-4a49-9da8-39b398e1aa27.png)


HikariCP의 연결 정보 외에 원하는 Connection Pool의 크기, 시간과 관련된 설정 등을 작성할 수 있다.

# 요약

-   Connection Pool 이란 JDBC 실행 과정 중에서 생성되어야 할 Connection 객체를 미리 만들어서 pool 이란 곳에서 저장을 해두는 기법이다
-   장점은 불필요한 과정(Connection객체를 생성,삭제)을 줄여서 성능을 높일 수 있다.
-   WAS에서 Connection Pool을 크게 설정하면 메모리 소모가 큰 대신 많은 사용자가 대기 시간이 줄어들고, 반대로 Connection Pool을 적게 설정하면 그 만큼 대기 시간이 길어진다. 따라서 사용량에 따라 적정량의 커넥션(Connection)객체를 생성해두어야 한다.