# 4. 예외

## 4.1. 초난감 예외처리

### 예외 블랙홀

```java
try{
    //TODO
}catch(SQLException e){
}
```

try/catch 블록에서 예외가 발생해도 코드를 계속 동작시킬 것이 아니라면 위 코드는 좋지 않다.
프로그램 실행 중 어디선가 오류가 있어 예외가 발생해도 어디에서 일으켰는지 파악할 수 없기 때문이다.

```java
catch(SQLException e){
    e.printStackTrace();
}
```

이 코드도 마찬가지이다. 출력해봤자 다른 로그나 메시지에 묻혀버리기 쉽상이다.
그리고 화면에 예외 로그를 출력한다고 해서 이것을 예외를 처리했다고 볼 수는 없다.
> 모든 예외는 적절하게 복구되든지 아니면 작업을 중단시키고 운영자 또는 개발자에게 분명하게 통보되어야 한다.

### 무책임한 throws

```java
public void method1() throws Exception{ method2(); }
public void method2() throws Exception{ method3(); }
public void method3() throws Exception{ ... }
```

모든 예외를 이렇게 Exception 처리하는 것도 문제가 있다.
- 단순 복사 붙여넣기식 Exception 이기 때문에, 어떤 예외적인 상황이 발생한다는 것인지 알 수 없다.
- 이 메서드를 호출한 다른 메서드도 똑같이 throws Exception 코드를 복사/붙여넣기 해야한다.

## 4.2 예외의 종류와 특징

### Error

에러는 시스템에 뭔가 비정상적인 상황이 발생했을 경우에 발생한다.
따라서 시스템 레벨에서 특별한 작업을 할 것이 아니라면 애플리케이션에서는 이런 에러에 대한 처리는 신경쓰지 않아도 된다.

### Exception과 체크 예외

Exception 클래스는 두 가지로 나뉜다.

- checked exception
- unchecked exception

#### unchecked exception

런타임 예외라고도 한다. 런타임 예외는 주로 프로그램의 오류가 있을 때 발생하도록 의도된 것이다.
대표적으로 NullPointerException 과 IllegalArgumentException 이 있다.

#### checked exception

IOException 이나 SQLException 을 비롯해서 예외적인 상황에서 던져질 가능성이 있는 것들 대부분은
checkedException 으로 만들어져 있다.

## 4.3 예외 처리 방법

### 예외 복구

네트워크가 불안해서 가끔 서버에 접속이 잘 안되는 환경에 있는 시스템이 있다고 가정한다.
원격 DB 서버에 접속하다 실패해서 SQLException 이 발생하는 경우에 재시도를 시도해볼 수 있다.
정해진 횟수만큼 재시도해서 실패했다면 예외 복구는 포기해야 한다.

### 예외 회피

예외처리를 자신이 담당하지 않고 자신을 호출한 쪽으로 던져버리는 것이다.
템플릿/콜백 패턴에서 콜백 오브젝트는 자신이 발생한 예외를 자신을 호출한 템플릿에게 던진다.
예외를 처리하는 것은 콜백이 처리해야할 문제가 아니기 때문이다.

하지만 템플리/콜백 처럼 긴밀한 연관 관계가 아닌 이상, 무책임하게 예외를 회피하면 문제가 발생할 수 있다.
DAO 에서 Exception 을 던진다고 해도, 이것을 호출하는 서비스와 컨트롤러에서 해당 예외를 처리할 수 있을 거라는
생각은 들지 않는다. 결국 컨트롤러나 서비스도 예외를 계속 던져서 이 예외는 결국 서버로 전달될 것이다.

예외를 회피하는 것은 예외를 복구하는 것보다 의도가 분명해야 한다.

### 예외 전환

예외를 복구해서 정상적인 상태로 만들 수 없기 때문에 예외를 메서드 밖으로 던지는 것이다.
예외 회피와 달리, 발생한 예외를 그대로 넘기는 게 아니라 적절한 예외로 전환해서 던진다는 특징이 있다.
예외 전환은 두 가지 특징이 있다.

- 내부에서 발생한 예외를 그대로 던지는 것이 그 예외 상황에 대한 적절한 의미를 부여해주지 못하는 경우,
  의미를 분명하게 해줄 예외로 바꿔주기 위해서다.

전환하는 예외에 원래 발생한 예외를 담아서 **중첩 예외**로 만드는 것이 좋다.

```java
public void(User user) throws DuplicateUserIdException, SQLException{
    try{
        //JDBC를 이용해 user 정보 추가하는 코드    
    }catch(SQLException e){
        if(e.getErrorCode() == MysqlErrorNumbers.EP_DUP_ENTRY)
            throw DuplicateUserIdException();
        else
            throw e;
    }
}
```

- 두 번째 전환 방법은 예외를 처리하기 쉽고 단순하게 만들기 위해 **포장** 한다.
  의미를 명확하게 하려고 다른 예외로 전환하는 게 아니라, 예외처리를 강제하는 체크 예외를 언체크 예외인 런타임 예외로 바꾸는 경우 사용한다.

예를 들면 EJBException 이 있다. EJB 컴포넌트 코드에서 발생하는 대부분의 체크 예외는 비즈니스 로직으로 볼 때 의미있는 예외가 아니다.
따라서 이런 경우에 런타임 예외인 EJBException으로 포장해서 던지는 편이 낫다.

> 대부분 서버환경에서는 애플리케이션 코드에서 처리하지 않고 전달된 예외들을 일괄적으로 다룰 수 있는 기능을 제공한다.
> 어차피 복구하지 못할 예외라면 코드에서는 런타임 예외로 포장해서 던져버리고, 예외처리 서비스 등을 이용해 자세한 로그를 남기고,
> 관리자에게는 메일 등으로 통보해주고, 사용자에게는 안내 메시지를 보여주는 식으로 처리하는게 바람직하다.


### 애플리케이션 예외

> 시스템 또는 외부의 예외상황이 원인이 아니라 애플리케이션 자체의 로직에 의해 의도적으로 발생시키고,
> 반드시 catch 해서 무엇인가 조치를 취하도록 요구하는 예외도 있다. 이런 예외들을 일반적으로 **애플리케이션 예외**라고 한다.

## 4.4 예외 전환

### JDBC의 한계

JDBC는 자바를 이용해 DB에 접근하는 방법을 추상화된 API 형태로 정의해놓고,
각 DB 업체가 JDBC 표준을 따라 만들어진 드라이버를 제공하게 해준다.
내부 구현은 다르지만 JDBC의 Connection, Statement, ResultSet 등의 표준 인터페이스를 통해
그 기능을 제공해주기 때문에 자바 개발자들은 표준화된 JDBC의 API에 익숙해지면 DB의 종류에 상관없이
일관된 방법으로 프로그램을 개발할 수 있다.

하지만 이런 JDBC에도 한계가 존재한다.

### 비표준 SQL

대부분의 DB는 표준을 따르지 않는 비표준 문법과 기능을 제공한다.
문제는 이런 비표준 문법을 사용했을 때 DAO가 해당 DB의 종류에 강력하게 귀속된다는 점이다.
이것은 DB의 변경 가능성을 고려해서 유연하게 만들어야 애플리케이션이라면 SQL은 제법 큰 걸림돌이 된다.

### 호환성 없는 SQLException의 DB 에러 정보

쿼리문 에러의 종류는 다양하지만, JDBC는 SQLException 예외 하나만을 지원한다.
```java
if(e.getErrorCode() == MysqlErrorNumbers.ER_DUP_ENTRY)
```

이와 같은 코드로 어떤 에러 코드인지 파악은 가능하지만, 해당 에러 코드는 MySQL 에 종속된 코드이기 때문에
DB 변경 시 해당 에러 코드를 전부 변경해줘야 한다.

결국 호환성 없는 에러 코드와 표준을 잘 따르지 않는 상태 코드를 가진 SQLException 만으로는 DB에 독립적인
유연한 코드를 작성하는 건 불가능에 가깝다.

## DB 에러 코드 매핑을 통한 전환

스프링은 DataAccessException 이라는 SQLException을 대체할 수 있는 런타임 예외를 정의하고 있다.
그리고 DataAccessException의 서브 클래스로 세분화된 예외 클래스들을 정의하고 있다.

- BadSqlGrammerException
- DataAccessResourceFailureException
- DataIntegrityViolationException

**문제는 DB마다 에러 코드가 각각이라는 점이다.**
그래서 스프링을 DB별 에러 코드를 분류해서 스프링이 정의한 예외 클래스와 매핑해놓은 에러 코드 매핑정보 테이블을 만들어두고
이를 이용한다.

## DAO 인터페이스와 DataAccessException 계층 구조

DataAccessException 은 JDBC에서만 사용되는 것이 아니라 JDBC외의 자바 데이터 액세스 기술에서 발생하는 예외도 적용된다.
JPA든 MyBatis든, DataAccessException은 의미가 같은 예외라면 종류와 상관없이 일관된 예외가 발생하도록 만들어준다.

### DAO 인터페이스와 구현의 분리

DAO를 굳이 따로 만들어서 사용하는 이유 :
> 데이터 액세스 로직을 담은 코드를 성격이 다른 코드에서 분리해놓기 위함
><br>또한 분리된 DAO는 전략 패턴을 적용해 구현 방법을 변경해서 사용할 수 있게 만들기 위해서이기도 함.

```java
public interface UserDao{
    public void add(User user);
}
```

기술에 독립적인 이상적인 DAO 인터페이스이지만 문제가 있다.
DAO에서 사용하는 데이터 액세스 기술의 API가 예외를 던지기 때문이다.
만약 JDBC API를 사용하는 UserDao 라면 SQLException 을 던지기 때문에 코드를 아래와 같이 수정해야 한다.

```java
public void add(User user) throws SQLException;
```

문제는 이렇게 인터페이스를 정의해놓으면 다른 데이터 액세스 기술로 DAO 를 구현했을 때 이 인터페이스를 사용할 수 없다.
```java
public void add(User user) throws PersistentException; // JPA
public void add(User user) throws HibernateException; // Hibernate
public void add(User user) throws JdoException; // JDO
```

데이터 액세스 기술마다 던지는 예외가 다르기 때문에 메서드의 선언이 달라진다는 문제가 발생한다.
다행히도 JDBC 를 체크 예외 대신 런타임 예외로 포장해서 던져줄 수 있기 때문에 DAO의 메서드는
처음 의도와 동일하게 작성해도 된다.
JPA, Hibernate, JDO 같은 기술들은 기본적으로 런타임 예외를 사용하기 때문에 가능한 일이다.

문제는 예외를 처리하려고 할 때 각자 다른 예외를 던지기 때문에, DAO를 사용하는 클라이언트 입장에서는
사용 기술에 따라서 예외 처리 방법이 달라져야 한다는 점이다.
즉 인터페이스로 추상화하고, 일부 기술에서 발생하는 체크 예외를 런타임 예외로 전환하는 것만으로는 부족하다.

### 데이터 액세스 예외 추상화와 DataAccessException 계층 구조

그래서 스프링은 자바의 다양한 데이터 액세스 기술을 사용할 때 발생하는 예외들을 추상화해서
**DataAccessException** 계층구조 안에 정리해놓았다.

JdbcTemplate 과 같이 스프링의 데이터 액세스 지원 기술을 이용해 DAO를 만들면 사용 기술에 독립적인
일관성 있는 예외를 던질 수 있다.

> 결국 인터페이스 사용, 런타임 예외 전환과 함께 DataAccessException 예외 추상화를 적용하면 데이터 액세스 기술과 구현 방법에 독립적인
이상적인 DAP를 만들 수 있다.

### DataAccessException 활용 시 주의사항

스프링을 활용하면 DB 종류나 데이터 액세스 기술에 상관없이 키 값이 중복되는 상황에서
DuplicateKeyException 예외가 던져질 것이라 생각하지만, 그렇지 않다.
이 예외는 JDBC를 이용하는 경우에만 발생하고 다른 기술들은 다른 예외가 발생한다.

그 이유는 JPA나 하이버네이트 같은 기술에서는 각 기술에서 재정의한 예외를 가져와
스프링이 최종적으로 DataAccessException으로 변환하는데, DB의 에러 코드와 달리 이런 예외들은
세분화되어 있지 않기 때문이다.

만약 사용하는 DAO에서 사용하는 기술의 종류와 상관없이 동일한 예외를 얻고 싶다면
DuplicatedUserIdException 과 같은 사용자 정의 예외를 정의해두고, 각 DAO 의 add() 메서드에서
좀 더 상세한 예외 전환을 해줄 필요가 있다.

#### SQLException을 DataAccessException으로 전환하는 방법

여러 방법을 지원하지만 가장 보편적인 방법은 DB 에러 코드를 이용하는 것이다.
SQLException을 코드에서 직접 전환하고 싶다면 SQLExceptionTranslator 인터페이스를 구현한
클래스 중에서 **SQLErrorCodeSQLExceptionTranslator** 를 사용하면 된다.

- DataSource 빈을 주입받도록 만든 UserDaoTest
```java
public class UserDaoTest{
    @Autowired
    UserDao dao;
    // 에러 코드 변환에 필요한 DB 종류를 알아내기 위해 현재 연결된 DataSource 정보가 필요하다.
    @Autowired
    DataSource dataSource;
}
```

- SQLException 전환 기능 테스트 코드
```java
@Test
public void sqlExceptionTranslate(){
    User user1 = new User("gyumee", "박성철", "springno1");
    dao.deleteAll();

    try {
        dao.add(user1);
        dao.add(user1);
    } catch (DuplicateKeyException ex) {
        SQLException sqlEx = (SQLException) ex.getRootCause();
        SQLExceptionTranslator set = new SQLErrorCodeSQLExceptionTranslator(this.dataSource);

        assertEquals(set.translate(null, null, sqlEx), DuplicateKeyException.class);
    }
}
```

- getRootCause() : 중첩된 SQLException 를 가져온다.
- translate() : SQLException 을 DataAccessException 타입의 예외로 변환해준다.

출처 : 토비의 스프링