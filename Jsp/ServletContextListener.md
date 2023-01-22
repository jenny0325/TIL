# ServletContextListener란?
- 웹 컨테이너는 웹 어플리케이션(컨텍스트)이 시작·종료되는 시점에 특정 클래스의 메서드를 실행할 수 있는 기능을 제공함
    - 이 기능을 통해 웹 어플리케이션 실행시 필요한 초기화 작업 또는 종료된 후 사용된 자원을 반환하는 작업 등을 수행
- 웹 어플리케이션 시작·종료시 특정한 기능을 실행하는 방법:
  - javax.servlet.ServletContextListener 인터페이스를 구현한 클래스를 작성
  - web.xml 파일에 1번에서 작성한 클래스를 등록
- javax.servlet.ServletContextListener: 웹 어플리케이션이 시작·종료될 때 호출할 메서드를 정의한 인터페이스
    - 인터페이스에 정의된 두 개의 메서드:
```java
public void contextInitialized(ServletContext sce) //웹 어플리케이션을 초기화할 때 호출
public void contextDestroyed(ServletContext sce) // 웹 어플리케이션을 종료할 때 호출
```
- 웹 어플리케이션이 시작·종료될 때 ServletContextListener 인터페이스를 구현한 클래스를 실행하려면, web.xml 파일에 <listener> 태그와 <listener-class> 태그를 사용해서 완전한 클래스 이름을 명시하기
```xml
<web-app ...>
    <listener>
        <listener-class>jdbc.DBCPInitListener</listener-class>
    </listener>

    <listener>
        <listener-class>chap20.CodeInitListener</listener-class>
    </listener>
    ...
</web-app>
```
- 한 개 이상의 <listener> 태그를 등록할 수 있고, 각 <listener> 태그는 반드시 한 개의 <listener-class> 태그를 자식으로 가져야함
- <listener-class> 태그는 웹 어플리케이션의 시작/종료 이벤트를 처리할 리스너 클래스의 완전한 이름을 값으로 갖음
- ServletContextListener 인터페이스에 정의된 두 메서드 모두 파라미터로 javax.ServletContextEvent 타입의 객체를 전달 받음
- ServletcontextEvent 클래스는 웹 어플리케이션 컨텍스트를 구할 수 있는 getServletContext() 메서드를 제공함
```java
  public ServletContext getServletContext()
```
- getServletContext() 메서드가 리턴하는 ServletContext 객체는 JSP의 application 기본 객체와 동일한 객체
    - 이것을 이용하면 web.xml 파일에 설정된 컨텍스트 초기화 파라미터를 구할 수 있음
    - 컨텍스트 초기화 파라미터는 <context-param> 태그를 사용하여 web.xml 파일에 설정함
```xml
<?xml version="1.0" encoding="UTF-8"?>
    <web-app ...>
        ...
            <context-param>
                <param-name>jdbcdriver</param-name>
                <param-value>com.mysql.jdbc.Driver</param-value>
            </context-param>
        ...
    </web-app>
```
- web.xml 파일에 설정한 초기화 파라미터 값을 구하는데 사용되는 getServletContext()의 메서드:
```java
/* 지정한 이름을 갖는 컨텍스트 초기화 파라미터의 값을 리턴
* 존재하지 않는 경우 null을 리턴
* name 파라미터에는 <param-name> 태그로 지정한 이름을 입력
*/
String getInitParameter(String name)

// 컨텍스트 초기화 파라미터의 이름 목록을 Enumeration 타입으로 리턴
java.util.Enumeration<String> getInitParameterNames()
```
- 컨텍스트 초기화 파라미터는 주로 웹 어플리케이션의 초기화 작업을 수행하는 데 필요한 값을 설정할 때 사용
- 예시 - ServletContextListener를 이용해서 DB 연동을 위한 커넥션 풀을 초기화하는 클래스
    - web.xml 파일에 커넥션 풀을 초기화할 때 사용할 컨텍스트 초기화 파라미터 설정
    - ServletContextListener 인터페이스를 구현한 클래스는 contextInitialized() 메서드에서 컨텍스트 초기화 파라미터를 이용해서 커넥션 풀을 초기화하는 데 필요한 값을 로딩
```xml
//chap20/WebContext/WEB-INF/web.xml

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    version="3.1">

    <listener>
        <listener-class>jdbc.DBCPInitListener</listener-class>
    </listener>

    <context-param>
        <param-name>poolConfig</param-name>
        <param-value>
        jdbcdriver=com.mysql.jdbc.Driver
        jdbcUrl=jdbc:mysql://localhost:3306/guestbook?characterEncoding=utf-8
        dbUser=jspexam
        dbPass=jsppw
        poolName=guestbook
        </param-value>
    </context-param>
</web-app>
```
- poolConfig 컨텍스트 초기화 파라미터를 이용해서 커넥션 풀을 초기화하는 코드

```java
//chap20/jdbc/DBCPInitListener.java
package jdbc;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import org.apache.commons.dbcp2.*;
import org.apache.commons.pool2.impl.GenericObjectPool;
import org.apache.commons.pool2.impl.GenericObjectPoolConfig;

import java.io.IOException;
import java.io.StringReader;
import java.sql.DriverManager;
import java.util.Properties;

public class DBCPInitListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        String poolConfig = sce.getServletContext().getInitParameter("poolConfig");
        Properties prop = new Properties();
        try {
            prop.load(new StringReader(poolConfig));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        loadJDBCDriver(prop);
        initConnectionPool(prop);
    }

    private void loadJDBCDriver(Properties prop) {
        String driverClass = prop.getProperty("jdbcdriver");
        try {
            Class.forName(driverClass);
        } catch (ClassNotFoundException ex) {
            throw new RuntimeException("fail to load JDBC Driver", ex);
        }
    }

    private void initConnectionPool(Properties prop) {
        try {
            String jdbcUrl = prop.getProperty("jdbcUrl");
            String username = prop.getProperty("dbUser");
            String pw = prop.getProperty("dbPass");

            ConnectionFactory connFactory = new DriverManagerConnectionFactory(jdbcUrl, username, pw);

            PoolableConnectionFactory poolableConnFactory = new PoolableConnectionFactory(connFactory, null);

            poolableConnFactory.setValidationQuery("select 1");

            GenericObjectPoolConfig poolConfig = new GenericObjectPoolConfig();
            poolConfig.setTimeBetweenEvictionRunsMillis(1000L * 60L * 5L);
            poolConfig.setTestWhileIdle(true);
            poolConfig.setMinIdle(4);
            poolConfig.setMaxTotal(50);

            GenericObjectPool<PoolableConnection> connectionPool = new GenericObjectPool<>(poolableConnFactory, poolConfig);
            poolableConnFactory.setPool(connectionPool);

            Class.forName("org.apache.commons.dbcp2.PoolingDriver");
            PoolingDriver driver = (PoolingDriver) DriverManager.getDriver("jdbc:apache:commons:dbcp:");
            String poolName = prop.getProperty("poolName");
            driver.registerPool(poolName, connectionPool);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    }
}
```
## 리스너의 실행 순서
- 웹 어플리케이션에는 한 개 이상의 리스너를 web.xml 파일에 등록할 수 있고, 이 경우에 contextInitialized() 메서드는 등록된 순서대로 실행되고, contextDestroyed() 메서드는 반대 순서로 실행됨

## 리스너에서 익셉션 처리하기
- contextInitialized() 메서드는 발생시킬 수 있는 checked 익셉션을 지정하지 않으므로, 익셉션을 발생시키려면 RuntimeException이나 그 하위 타입의 익셉션을 발생시켜야함

```java
public void contextInitialized(ServletContextEvent sce){
    String poolConfig=sce.getServletContext().getInitParameter("poolConfig");
    Properties prop=new Properties();
    try {
        prop.load(new StringReader(poolConfig));
    } catch(IOException e) {
        throw new RuntimeException(e);
    }
    loadJDBCDriver(prop);
    initConnectionPool(prop);
}
```

## 어노테이션을 이용한 리스너 등록
- @WebListener 어노테이션을 리스터 클래스에 적용하면, web.xml 파일에 등록하지 않아도 자동으로 리스너로 등록됨
    - 서블릿 3.0 버전부터 사용 가능
```java
import javax.servlet.annotation.WebListener;

@WebListener
public class CListener implements ServletContextListener {
      ...
}
```
