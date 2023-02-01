# 불변객체

---

먼저 위키에 있는 내용을 보면 다음과 같다.

```bash
객체 지향 프로그래밍에 있어서 불변객체(immutable object)는 생성 후 그 상태를 바꿀 수 없는 객체를 말한다.
...
또, 경우에 따라서는 내부에서 사용하는 속성이 변화해도 외부에서 그 객체의 상태가 변하지 않은 것처럼 보인다면 불변 객체로 보기도 한다.
```

제일 중요한 개념은 **`"생성 한 뒤 내부 상태가 변경되지 않는다."`**이다.

## String

**Java**의 대표적인 불변 객체로는 `String`이 있다.

보통 우리는 String을 선언할 때 다음과 같이 선언한다.

```java
String str = "ab";
```

그럼 다음과 같이 문자열을 추가하게 된다면 어떻게 될까?

```java
str = str + "cd";
```

`str`이 변경됐을꺼라 생각할 수 있지만, 사실 `"ab"`라는 `String` 객체는 그대로 남아있고, `"abcd"`라는 새로운 객체가 `str` 변수에 할당된다.

> 💡**Java**에서 **`String`**은 특이하게 **Heap** 영역에서 **String Constant Pool**이 존재하는데, `String`을 **Literal**로 생성하면 이 영역에 저장되어 재사용된다. 하지만, `new` 생성자로 생성하면 **Constant Pool**을 사용하지 않고, 일반적인 **Heap** 영역에 생성하여 재사용하지 않게된다.
>

# 불변객체의 장점?

---

불변객체를 어떻게 만드는지 알기 전에 어떤 장점이 있는지 먼저 알아보자.

## Thread-Safe 하다.

멀티 쓰레드 환경에서 동기화 이슈가 발생하는 이유는 공유 자원에 대한 덮어쓰기가 발생하기 때문이다.

이를 불변 객체를 이용하여 로직을 구성하게 되면, 동기화에 대한 이슈를 해결할 수 있다.

쓰레드마다 새로운 객체를 생성하여 접근하기 때문에 덮어쓰기가 발생하지 않기 때문이다.

또한 `synchronized`를 사용하여 동기화하지 않아도 되기 때문에 성능도 좋아진다.

## 객체에 대한 신뢰도가 높아진다. (=Side Effect를 최소화할 수 있다.)

객체가 어떤 상태값으로 생성된 뒤에 변화가 없을 것이기 때문에 상태에 대한 보장을 받을 수 있고, 이후에 사용 시에도 믿고 사용할 수 있다.

만약 가변 객체(`Setter`가 있는)라면 시시때때로 변화하는 객체라면 특정 시점에 객체의 상태를 예측하기 힘들진다.

상태값을 일일히 확인하기 위해 메소드를 일일히 살펴봐야 하고 이는 유지보수에 큰 시간을 할애하게 만든다.

불변 객체는 상태의 변경이 불가능하게 하기 위해 객체의 생성과 사용이 상당히 제한된다.

이는 메소드를 순수함수로 만들어줄 것이며, 유지보수성이 높은 코드가 되도록 유도할 것이다.

> **💡순수함수란?**
동일한 인자가 들어갈 경우 반드시 같은 값을 **return**하는 함수를 말한다.
>

## 방어적 복사를 만들 필요가 없어진다.

먼저, 방어적 복사란 무엇일까?

객체의 특정 상태를 `getter` 메소드 등으로 확인하거나 접근할 때 객체 내부적으로 복사본을 새로 만들어 반환하는 코드를 말한다.

객체의 특정 상태 값을 변경할 때 안전하게 원본 객체를 보존하기 위한 방법으로 사용한다.

하지만 불변객체에서는 이런 방어적 복사 코드를 사용할 수고를 덜어준다.


## GC의 성능을 높일 수 있다.

사실 굉장히 의아스러운 점이었다.

**`"새로운 객체가 많이 만들어질 수록 GC의 성능은 떨어지지 않나?"`**라고 생각했다.

먼저 오라클 공식 문서에 따르면 다음과 같이 적혀 있다.

```
Programmers are often reluctant to employ immutable objects, 
because they worry about the cost of creating a new object as opposed to updating an object in place. 
The impact of object creation is often overestimated, 
and can be offset by some of the efficiencies associated with immutable objects. 
These include decreased overhead due to garbage collection, 
and the elimination of code needed to protect mutable objects from corruption.
```

**“프로그래머는 객체 생성에 대한 비용 때문에 불변을 꺼려하지만 불변을 이용한 효율로 상쇄될 수 있다”**

라고 적혀있다.

그러면 어떻게 이게 가능한지 보자.

### 불변객체

```java
public class ImmutableWrapper {
    private final Object obj;
    public ImmutableWrapper(Object obj) {
        this.obj = obj;
    }
    public Object getObj() {
        return this.obj;
    }
}
```

위와 같이 불변객체를 생성해내는 `ImmutableWrapper` 클래스가 있다.

`ImmutableWrapper` 내부에 `obj`에 할당된 객체는 `ImmutableWrapper`가 **GC**에 의해 제거되기 전까지 반드시 참조되어 살아있을 것이다.

다시 말하면 `ImmutableWrapper`가 살아있는 이상 내부에 있는 `obj`는 **GC** 스캔 대상에서 제외된다.

즉, 불변객체를 사용하면 **GC**의 스캔 범위 및 스캔 빈도수가 줄어들게 되어 **GC**의 성능을 높이는 결과를 가져오게 된다.

만약 위의 `ImmutableWrapper`를 가변객체로 사용한다면 어떻게 될까?

### 가변객체

```java
public class MutableWrapper {
    private Object obj;
    public void setObj(Object obj) {
        this.obj = obj;
    }
    public Object getObj() {
        return this.obj;
    }
}
```

`setter`를 통해 A라는 객체를 세팅하고, 후에 B라는 객체를 세팅하게 되면 A 객체는 더 이상 참조하지 않기 때문에 GC 스캔 대상이 된다.

이렇게 스캔범위 및 스캔 빈도수가 늘어나게 되고 결국 **GC**의 성능이 떨어지게 된다.

그리고 중요한 사실이 있는데, **GC**는 새롭게 생성된 객체는 금방 죽는다는 **Weak Generational Hypothesis** 가설에 맞춰 설계되었다.

가변객체는 보통 하나의 객체를 가지고 상태를 변화하는 형태로 코드가 작성되기 때문에 오래 살아 있고 불변객체는 보통 생명주기가 짧기 때문에 **GC**가 처리하는데 더 적합하다.

# 불변 객체 규칙

그럼 불변객체는 어떻게 만들까?

**Java**에선 다음과 같이 4가지 규칙을 얘기하고 있다.

1. 클래스를 `final`로 선언하라.
2. 모든 클래스 변수를 `private`와 `final`로 선언하라.
3. 객체를 생성하기 위한 생성자나 팩토리 메소드를 추가하라.
4. 참조에 의해 변경 가능성이 있는 경우 방어적 복사를 이용하여 전달하라.

```java
public final class Team {

    private final String name;
    private final int memberCount;
    private final List<User> members;

    private Team(String name, int memberCount, List<User> members) {
        this.name = name;
        this.memberCount = memberCount;
        this.members = members;
    }

    public static Team of(String name, int memberCount, List members) {
        return new Team(name, memberCount, members);
    }

    public List getMembers() {
        return Collections.unmodifiableList(this.members);
    }

    // 그외 getter
}
```

위의 예시는 `Team`이라는 클래스이며 멤버 변수는 final로 선언되어 초기화 이후 변경할 수 없게 했다

초기화는 **static factory method**를 통해 객체를 초기화 하도록 했다.

그리고 `getter` 메소드를 통해 멤버 변수를 읽을 수만 있다.

특이한 점은 `getMembers`에서 `Collections.unmodifiableList`를 사용하여 방어적 복사를 진행한 것이다.

그대로 `members`를 반환할 경우 `members.add` 등을 사용하여 `members`의 상태를 변경할 수 있기 때문에 방어적 복사를 사용했다.

Java에서는 생성자를 선언하지 않으면 기본 생성자가 자동으로 생성되는데, 그러면 다른 클래스에서 해당 객체를 자유롭게 호출할 수 있다. 그렇기 때문에 내부 생성자를 만드는 대신 정적 팩토리 메소드를 통해 객체를 생성하도록 강요하는 것이 좋다.

또한 배열이나 다른 객체 또는 컬렉션은 참조가 전달되어 수정가능성이 있다. 그렇기 때문에 참조를 통해 변경이 가능한 경우에는 방어적 복사를 통해 값을 반환해야 한다. 마지막으로 클래스의 변수에 가능하다면 final을, final이 불가능하다면 Setter를 최소화하도록 하자.


# 마무리

---

이펙티브 자바에선 다음과 같은 문구가 있다.

```java
클래스들은 가변적이여야 하는 매우 타당한 이유가 있지 않는 한 반드시 불변으로 만들어야 한다.
만약 클래스를 불변으로 만드는 것이 불가능하다면, 가능한 변경 가능성을 최소화하라.
```
이를 명심하고 개발하도록 하자.

# 📌참고

---

[[Java] 불변 객체(Immutable Object) 및 final을 사용해야 하는 이유](https://mangkyu.tistory.com/131)

[[Java] Immutable Object(불변객체)](https://velog.io/@conatuseus/Java-Immutable-Object%EB%B6%88%EB%B3%80%EA%B0%9D%EC%B2%B4)