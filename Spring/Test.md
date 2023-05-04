# 스프링 테스트

# 1. JUnit 프레임워크

## 1.1 픽스처

> 테스트를 수행하는 데 필요한 정보나 오브젝트를 **픽스처**라고 한다.

일반적으로 픽스처는 여러 테스트에서 반복적으로 사용되기 때문에 @Before (Junit5 에서는 @BeforeEach)
메서드를 이용해 생성해두면 편리하다.

## 2. 스프링 테스트 적용

### 2.1 테스트를 위한 애플리케이션 컨텍스트 관리

애플리케이션 컨텍스트는 모든 테스트 코드에서 사용한다.
하지만 Junit 은 매번 테스트 클래스의 오브젝트를 새로 생성한다.
테스트는 가능한 한 독립적으로 매번 새로운 오브젝트를 생성해서 사용하는 것이 원칙이지만,
애플리케이션 컨텍스트처럼 생성에 많은 시간과 자원이 소모되는 경우 테스트 전체가 공유하는 것이 더 낫다.
당연히 스프링은 이러한 상황을 위한 기능을 제공한다.

#### 2.1.1 스프링 테스트 컨텍스트 프레임워크 적용

Junit5 기준으로 아래와 같이 작성한다.

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(locations = "/applicationContext.xml")
public class UserDaoTest {
    private UserDao dao;

    @Autowired
    private ApplicationContext context;

    @BeforeEach
    public void setUp() {
        this.dao = this.context.getBean("userDao", UserDao.class);
    }
    ...
}
```

- @ExtendWith : Junit이 테스트를 실행하는 중에 테스트가 사용할 애플리케이션 컨텍스트를 만들고 관리하는 작업을 진행한다.
- @ContextConfiguration : 자동으로 만들어줄 애플리케이션 컨텍스트의 설정파일 위치를 지정한다.

##### 테스트 클래스의 컨텍스트 공유

여러 개의 테스트 클래스가 있는데 모두 같은 설정파일을 가진 애플리케이션 컨텍스트를 사용한다면,
스프링은 테스트 클래스 사이에서도 애플리케이션 컨텍스트를 공유하게 만들어준다.
따라서 수백 개의 테스트 클래스를 만들어도 동일한 설정파일을 사용한다고 하면,
테스트 전체에 걸쳐 단 한 개의 애플리케이션 컨텍스트만 만들어져 사용된다.
따라서 테스트의 성능이 크게 향상된다.

##### @Autowired

@Autowired 가 붙은 인스턴스 변수가 있으면, 테스트 컨텍스트 프레임워크는 **변수 타입과 일치하는
컨텍스트 내의 빈을 찾는다.**

앞의 테스트 코드에서는 applicationContext.xml 파일에 정의된 빈이 아니라,
ApplicationContext 라는 타입의 변수에 @Autowired 를 붙였는데 DI가 됐다.
그 이유는, 스프링 애플리케이션 컨텍스트는 초기화할 때 자기 자신도 빈으로 등록하기 때문이다.
따라서 애플리케이션 컨텍스트에는 ApplicationContext 타입의 빈이 존재하는 셈이고 DI도 가능하다.

#### 2.1.2 DI와 테스트

DI를 적용하면서 테스트를 진행하는 데는 3가지 방식이 있다.

- @DirtiesContext
- 테스트용 applicationContext 파일 새로 생성
- 컨테이너 없는 DI 테스트

##### @DirtiesContext

@DirtiesContext 애노테이션은 테스트 메서드에서 애플리케이션 컨텍스트의 구성이나 상태를 변경한다는 것을 테스트 컨텍스트 프레임워크에 알려준다.
테스트 컨텍스트는 이 애노테이션이 붙은 테스트 클레스에는 애플리케이션 컨텍스트 공유를 허용하지 않는다.
이것은 의존관계를 강제로 DI 해서 다음 테스트에서 벌어질 문제들을 차단하기 위함이지만,
애플리케이션 컨텍스트가 테스트 클래스마다 매번 생성된다는 단점이 있다.

##### 테스트용 applicationContext 파일 새로 생성

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(locations = "/test-applicationContext.xml")
```

테스트용 애플리케이션 설정파일을 만들면, 기존 DI의 장점을 최대화 할 수 있다.
테스트 코드에서는 애플리케이션 컨텍스트를 기존처럼 공유하기 때문에 테스트 속도가 향상되기 때문이다.

##### 컨테이너 없는 DI 테스트

컨테이너 없는 DI 테스트도 가능하다.
예를 들어, DAO 테스트를 할 때는 DB에 정보를 잘 등록하고 잘 가져오는지 확인만 하면 된다.
굳이 생성에 많은 비용이 드는 애플리케이션 컨텍스트 생성이 필요없는 것이다.

```java
import javax.sql.DataSource;

public class CustomDaoTest {
    UserDao dao; // Autowired 는 필요없다.
    ...

    @BeforeEach
    public void setUp() {
        dao = new CustomDao();
        DataSource dataSource = new SingleConnectionDataSource(
                "jdbc:mysql://localhost/testdb", "spring", "password", true);
        );
        dao.setDataSource(dataSource);
    }
}
```

테스트를 위한 DataSource 를 직접 만드는 번거로움은 있지만 애플리케이션 컨텍스트를 아예 사용하지 않기 때문에 코드는 더 단순해진다.
SingleConnectionDataSource 는 생성 비용이 매우 적기 때문에 테스트 시간도 그만큼 줄일 수 있다.

##### DI 를 이용한 테스트 방법 선택

1. 항상 스프링 컨테이너 없이 테스트할 수 있는 방법을 우선적으로 고려한다.
2. 여러 오브젝트와 복잡한 의존관계를 갖고 있는 오브젝트를 테스트해야 할 경우, 스프링 설정을 이용한 DI 방식의 테스트를 이용한다.
3. 때로는 예외적인 의존관계를 강제로 구성해서 테스트해야할 때 수동 DI를 통해서 테스트 하는 방법을 고려한다.
   이 때 @DirtiesContext 애노테이션은 반드시 붙여야 한다.

## 2. 버그 테스트

### 2.1 개요

버그테스트란 코드에 오류가 있을 때 그 오류를 가장 잘 드러내줄 수 있는 코드를 말한다.
버그 테스트는 일단 실패하도록 만들어야 한다. 버그가 원인이 되서 테스트가 실패하는 코드를 만드는 것이다.
그러고 나서 버그 테스트가 성공할 수 있도록 애플리케이션 코드를 수정한다.
테스트가 성공하면 버그는 해결된 것이다.

#### 동등분할(equivalence partitioning)

같은 결과를 내는 값의 범위를 구분해서 각 대표 값으로 테스트를 하는 방법

#### 경계값 분석(boundary value analysis)

에러는 동등분할 범위의 경계에서 주로 발생한다는 특징을 이용해서 경계의 근처에 있는 값을 이용해 테스트하는 방법

출처 : 토비의 스프링