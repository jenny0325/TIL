# 1장. 오브젝트와 의존 관계

## 1. 제어의 역전(IoC)

### 1.1 오브젝트 팩토리

**UserDao 클래스**와 **UserDao 클래스의 객체 생성 방법을 결정하고 리턴해주는 역할**을 하는 클래스로 나눈다.
이 때 후자는 객체 생성이란 책임에 집중하는 **오브젝트 팩토리(Object Factory)** 라고 부른다.

- **팩토리 클래스 예제**

```java
public class DaoFactory{
    //어떤 connectionMaker를 사용할 지 결정하고 그 객체를 주입시키고, 리턴한다.
    public UserDao userDao(){
        ConnectionMaker connectionMaker = new DConnectionMaker();
        UserDao userDao = new UserDao(connectionMaker);
        return userDao;
    }
}
```

```java
public class UserDaoTest{
    public static void main(String[] args) {
        UserDao dao = new DaoFactory.userDao();
    }
}
```

#### 1.1.1 팩토리 사용의 이점

애플리케이션의 컴포넌트 역할을 하는 객체(UserDao)와 애플리케이션의 구조를 결정하는 오브젝트(DaoFactory)를 분리했다는 것에 가장 의미가 있다.

#### 1.1.2 오브젝트 팩토리 활용

DaoFactory 에 다른 DAO의 생성 기능을 넣으면 다음과 같다.

```java
public class DaoFactory{
    public UserDao userDao(){
        return new userDao(new DConnectionMaker());
    }

    public UserDao accountDao(){
        return new userDao(new DConnectionMaker());
    }

    public UserDao messageDao(){
        return new userDao(new DConnectionMaker());
    }
}
```

new DconnectionMaker() 코드가 반복되므로 이 코드를 따로 분리시킬 수 있다.

```java
public class DaoFactory{
    public UserDao userDao(){
        return new userDao(connectionMaker());
    }

    public UserDao accountDao(){
        return new userDao(connectionMaker());
    }

    public UserDao messageDao(){
        return new userDao(connectionMaker());
    }
    
    public ConnectionMaker connectionMaker(){
        return new DconnectionMaker();
    }
}
```

#### 1.1.3 제어권의 이전을 통한 제어관계 역전

> 프로그램의 일반적인 흐름 구조를 뒤바꾸는 것이라고 설명할 수 있다.

일반적의 흐름 구조라 함은 다음과 같다.

1. main() 메서드와 같은 엔트리 포인트에서 다음에 사용할 객체를 결정한다.
2. 결정한 객체를 생성한다.
3. 만들어진 객체에 있는 메서드를 호출한다.
4. 그 객체 메서드 안에서 다음에 사용할 것을 결정하고 호출하는 식의 작업을 반복한다.

이러한 작업에서 각 객체는 프로그램의 흐름을 결정하고, 사용할 객체를 구성하는 작업에 능동적으로 참여한다.
제어의 역전이란 이런 제어 흐름의 개념을 뒤바꾸는 것이다.

제어의 역전에서 객체는 자신이 사용할 객체를 스스로 결정하지 않고, 당연히 생성하지도 않는다.
또 자신도 어떻게 만들어지고 어떻게 생성되는 지를 알 수 없다.
이것은 **모든 제어 권한을 자신이 아닌 다른 대상에게 위임하기 때문이다.**

제어의 역전이 적용된 사례는 여러가지가 있다.

- 서블릿
- 템플릿 메서드 패턴
- 프레임워크

**1. 서블릿**

서블릿을 개발해서 서버에 배포할 수 있지만, 그 실행을 개발자가 직접 제어할 수 있는 방법은 없다.
대신 서블릿에 제어 권한을 가진 **컨테이너**가 적절한 시점에 서블릿 클래스의 객체를 만들고 그 안의 메서드를 호출한다.

**2. 템플릿 메서드 패턴**

제어권을 상위 템플릿에게 넘기고 자신은 필요할 때 호출되어 사용되도록 한다는, 제어 역전의 개념이 담겨 있다.

**3. 프레임 워크**

프레임워크는 애플리케이션 코드가 프레임워크에 의해 사용된다.
보통 프레임워크 위에 개발한 클래스를 등록하고, 프레임워크가 흐름을 주도하는 중에 개발자가 만든 애플리케이션 코드를 사용하도록 만드는 방식이다.


**4. 예시 코드**

위의 UserDao 와 DaoFactory 에도 IoC가 적용되어 있다.
원래라면 UserDao 가 ConnectionMaker 의 구현체를 생성하는 책임을 떠맡고 있었지만, 그 책임을 DaoFactory 가 맡고 있다.
즉, 자신이 어떤 connectionMaker 구현 클래스를 만들고 사용할지를 결정할 권한을 DaoFactory 에게 위임했다.


#### 1.1.4 오브젝트 팩토리를 이용한 스프링 IoC

- 빈(Bean)

> 스프링에서 스프링이 제어권을 가지고 직접 만들고 관계를 부여하는 객체를 **빈**이라고 부른다.
> 스프링 빈은 스프링 컨테이너가 생성과 관계설정, 사용 등을 제어해주는 제어의 역전이 적용된 객체(오브젝트)를 말한다.

- 빈 팩토리(Bean Factory)

> 빈의 생성과 관계설정 같은 제어를 담당하는 IoC 객체를 **빈 팩토리**라고 부른다.

- 애플리케이션 컨텍스트(Application Context)

> 빈 팩토리 기능을 조금 더 확장시킨 것이며, 애플리케이션 컨텍스트도 IoC 방식을 따라 만들어진 일종의 빈 팩토리이다.

애플리케이션 컨텍스트는 어떤 클래스의 객체를 생성할 것인지, 어디에서 사용하도록 연결해줄 것인지에 대한 정보를 담고 있지 않다.
다만 이런 정보를 가지는 어떤 **설정정보**를 활용하는 범용적인 IoC 엔진이라고 볼 수 있다.


### 1.2 애플리케이션 컨텍스트(Application Context)

스프링 애노테이션을 이용하면 이전 코드를 다음과 같이 변경가능하다.

```java
@Configuration
public class DaoFactory {
    @Bean
    public UserDao userDao() {
        return new UserDao(connectionMaker());
    }

    @Bean
    public ConnectionMaker connectionMaker() {
        return new SimpleConnectionMaker();
    }
}
```
```java
public class UserDaoTest {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(DaoFactory.class);
        UserDao dao = context.getBean("userDao", UserDao.class);
    }
}
```

- @Configuration : 애플리케이션 컨텍스트 또는 빈 팩토리가 사용할 설정정보라는 표시
- @Bean : 오브젝트 생성을 담당하는 IoC 용 메서드라는 표시

애플리케이션 컨텍스트는 ApplicationContext 인터페이스를 구현한다.
그리고 ApplicationContext 는 BeanFactory 인터페이스를 상속했으므로 일종의 빈 팩토리라고도 부를 수 있다.

![](https://i.imgur.com/Uf9J8vi.png)

애플리케이션 컨텍스트는 애플리케이션에서 IoC를 적용해서 관리할 모든 오브젝트에 대한 생성과 관계 설정을 담당한다.
애플리케이션 컨텍스트를 사용해서 얻을 수 있는 이점은 다음과 같다.

- **클라이언트는 구체적인 팩토리 클래스를 알 필요가 없다.**

오브젝트 팩토리가 아무리 많아져도 이를 알아야하거나 직접 사용할 필요가 없다.
애플리케이션 컨텍스트를 이용해 일관된 방식으로 원하는 오브젝트를 가져올 수 있다.
또한 애노테이션 방식이 아닌 XML 방법을 선택해 IoC 설정정보를 만들 수도 있다.

- **애플리케이션 컨텍스트는 종합 IoC 서비스를 제공해준다.**

애플리케이션 컨텍스트는 오브젝트 생성, 타 오브젝트와의 관계설정만 하는 것이 아닌,
오브젝트 생성 방식, 자동 생성, 오브젝트에 대한 후처리, 정보의 조합, 설정 방식의 다변화, 인터셉팅 등 오브젝트를 효과적으로 활용할 수 있는
다양한 기능을 제공한다.

- **애플리케이션 컨텍스트는 빈을 검색하는 다양한 방법을 제공한다.**

애플리케이션 컨텍스트는 getBean() 메서드를 이용해 빈을 검색한다.
타입만으로 빈을 검색하거나 특별한 애노테이션 설정이 되어 있는 빈을 찾을 수도 있다.

#### 1.2.1 스프링 IoC의 용어 정리

- 빈(Bean)

> 빈 또는 빈 오브젝트는 스프링이 IoC 방식으로 관리하는 오브젝트라는 뜻이다.

- 빈 팩토리(Bean Factory)

> 스프링의 IoC를 담당하는 핵심 컨테이너를 가리킨다.

- 애플리케이션 컨텍스트(Application Context)

> 빈 팩토리를 확장한 IoC 컨테이너이다. 스프링이 제공하는 각종 부가 서비스를 추가로 제공한다.

- 설정정보

> 애플리케이션 컨텍스트 또는 빈 팩토리가 IoC를 적용하기 위해 사용하는 메타정보를 뜻한다.

- 컨테이너 또는 IoC 컨테이너

> IoC 방식으로 빈을 관리한다는 의미에서 애플리케이션 컨텍스트 또는 빈 팩토리를 컨테이너 또는 IoC 컨테이너라고 부른다.
> 컨테이너 말 자체가 IoC 개념을 담고 있으므로 이름이 긴 애플리케이션 컨텍스트보다 스프링 컨테이너라고 부르는 걸 선호하는 사람이 있다.

컨테이너라는 말은 애플리케이션 컨텍스트보다 더 추상적인 개념이다.
애플리케이션 컨텍스트는 ApplicationContext 인터페이스를 구현한 오브젝트를 말하기도 하는데,
하나의 애플리케이션에서 보통 여러 개의 ApplicationContext 가 만들어진다.
이 모든 ApplicationContext 를 스프링 컨테이터라고 부를 수 있다.

### 2. 출처

> 토비의 스프링 1권