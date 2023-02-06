# abstract 키워드 - 추상 메서드와 추상 클래스

- 추상 메서드(Abstract Method): 선언부는 있는데 구현부가 없는 메서드

- 추상 클래스: 인스턴스, 즉 객체를 만들 수 없다. 즉, new를 사용할 수 없다.

추상 메서드는 하위 클래스에게 메서드의 구현을 강제한다. 오버 라이딩 강제.

추상 메서드를 포함하는 클래스는 반드시 추상 클래스(Abstract Class)여야 한다.

# 생성자

> 객체를 생성하는 메서드 (객체 생성자 메서드) = 생성자

- 개발자가 아무런 생성자도 만들지 않으면 자바는 인자가 없는 기본 생성자를 자동으로 만들어준다.
- 인자가 있는 생성자를 하나라도 만든다면 자바는 기본 생성자를 만들어 주지 않는다.

# 클래스 생성 시의 실행 블록, static 블록

static 블록에서 사용할 수 있는 속성과 메서드는 당연히 static 멤버 뿐. 객체 멤버는 클래스가 static 영역에 자리 잡은 후에 객체 생성자를 통해 힙에 생성된다.  
클래스의 static 블록이 실행되고 있을 때는 해당 클래스의 객체는 하나도 존재하지 않기 때문에 static 블록에서는 객체 멤버에 접근할 수 없는 것이다.

클래스 정보는 해당 클래스가 코드에서 맨 처음 사용될 때 T 메모리의 스태틱 영역에 로딩되며, 이때 단 한번 해당 클래스의 static 블록이 실행된다.

#### 클래스가 제일 처음 사용될 때

- 클래스의 정적 속성을 사용할 때
- 클래스의 정적 메서드를 사용할 때
- 클래스의 인스턴스를 최초로 만들 때

인스턴스 초기화 블록은 객체 생성자가 실행되기 전에 먼저 실행된다.

# final 키워드

최종이라는 의미를 지닌 final 키워드와 함께 쓰인 클래스, 변수, 메서드는 각각 상속, 변경, 오버라이딩을 금지한다.

# instanceof 연산자

만들어진 객체가 특정 클래스의 인스턴스인지 물어보는 연산자. 결과로 true 또는 false를 반환한다.

```java
package instanceOf01;

class 동물 {

}

class 조류 extends 동물 {

}

class 펭귄 extends 조류 {

}

public class Driver {
    public static void main(String[] args) {
        동물 동물객체 = new 동물();
        조류 조류객체 = new 조류();
        펭귄 펭귄객체 = new 펭귄();

        System.out.println(동물객체 instanceof 동물);

        System.out.println(조류객체 instanceof 동물);
        System.out.println(조류객체 instanceof 조류);

        System.out.println(펭귄객체 instanceof 동물);
        System.out.println(펭귄객체 instanceof 조류);
        System.out.println(펭귄객체 instanceof 펭귄);

        System.out.println(펭귄객체 instanceof Object);
    }
}
```

```java
package instanceOf02;

class 동물 {

}

class 조류 extends 동물 {

}

class 펭귄 extends 조류 {

}

public class Driver {
    public static void main(String[] args) {
        동물 동물객체 = new 동물();
        동물 조류객체 = new 조류();
        동물 펭귄객체 = new 펭귄();

        System.out.println(동물객체 instanceof 동물);

        System.out.println(조류객체 instanceof 동물);
        System.out.println(조류객체 instanceof 조류);

        System.out.println(펭귄객체 instanceof 동물);
        System.out.println(펭귄객체 instanceof 조류);
        System.out.println(펭귄객체 instanceof 펭귄);

        System.out.println(펭귄객체 instanceof Object);
    }
}
//객체 참조 변수의 타입이 아닌 실제 객체의 타입에 의해 처리됨.
```
# package 키워드

네임스페이스(이름공간)를 만들어주는 역할

# interface 키워드와 implements 키워드

인터페이스는 public 추상 메서드와 public 정적 상수만을 가질 수 있다.

자바 8부터는 디폴트 메서드라고 하는 객체 구상 메서드와 정적 추상 메서드를 지원할 수 있게 언어 스펙이 변경됨

# this 키워드

객체가 자신을 지칭할 때 쓰는 키워드

- 지역 변수와 속성(객체 변수, 정적 변수)의 이름이 같은 경우 지역 변수가 우선한다.
- 객체 변수와 이름이 같은 지역 변수가 있는 경우 객체 변수를 사용하려면 this를 접두사로 사용한다.
- 정적 변수와 이름이 같은 지역 변수가 있는 경우 정적 변수를 사용하려면 클래스명을 접두사로 사용한다.

# super 키워드

바로 위 상위 클래스의 인스턴스를 지칭하는 키워드