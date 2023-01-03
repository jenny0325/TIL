# 제네릭(Generic)
- 타입 형 변환에서 발생할 수 있는 문제점을 컴파일 할 때 없애기 위해 만들어졌다.
- 꺽쇠 안에 선언된 이름은 아무 이름으로 해도 상관없다. 가상의 타입 이름이다.
```java
public class CastingGenericDTO<T> implements Serializable {
    private T object;
    public void setObject(T obj) {
        this.object = obj;
    }
    public T getObject() {
        return object;
    }
}
```
```java
public class GenericSample {
    public static void main(String[] args){
        GenericSample sample = new GenericSample();
        sample.checkGenericDTO();
    }
public void checkGenericDTO() {
    CastingGenericDTO<String> dto1 = new CastingGenericDTO<String>();
    dto1.setObject(new String());
    CastingGenericDTO<StringBuffer> dto2 = new CastingGenericDTO<StringBuffer>();
    dto2.setObject(new StringBuffer());
    CastingGenericDTO<StringBuilder> dto3 = new CastingGenericDTO<StringBuilder>();
    dto3.setObject(new StringBuilder());

    String temp1 = dto1.getObject();
    StringBuffer temp2 = dto2.getObject();
    StringBuilder temp3 = dto3.getObject();
    }
}
```

# 제네릭 타입 이름 정하기
- 제네릭 타입 클래스 선언시 꺾쇠 안에 들어가는 이름 기본 규칙 
  - E : 요소
  - K : 키 
  - N : 숫자 
  - T : 타입 
  - V : 값 
  - S, U, V : 두 번째, 세 번째, 네 번째에 선언된 타입
- 타입 대신에 ? 를 적어주면 어떤 타입이 제네릭 타입이 되더라도 상관 없다.
  - wildcard 타입이라고 한다.
  - 값을 받을 때는 Object로 처리한다.
  - 값을 할당할수는 없으며 조회만 가능하다.
# 제네릭 타입 범위 지정  
- ? extends 타입 = Bounded Wildcards 타입
- 타입을 상속받은 모든 클래스를 사용할 수 있다.
- 어떤 객체를 wildcard로 선언하고, 그 객체의 값은 가져올 수는 있지만, wildcard로 객체를 선언했을 때에는 특정 타입으로 값을 지정하는 것은 불가능하다.  
= 값을 할당할수는 없으며 조회만 가능하다
```java
public class Car {
    protected String name;
    public Car(String name){
        this.name = name;
    }
    public String toString() {
        return "Car name = " + name;
    }
}

class Bus extends Car {
    public Bus(String name){
        super(name);
    }
    public String toString(){
        return "Bus name = " + name;
    }
}

public class WildcardGeneric<W>{
    W wildcard;
    public void setWildcard(W wildcard){
        this.wildcard = wildcard;
    }
    public W getWildcard(){
        return wildcard;
    }
}

public class CardWildcardSample{
    public static void main(String args[]){
        CardWildcardSample sample = new CardWildcardSample();
        sample.callBoundedWildcardMethod();
    }
    public void callBoundedWildcardMethod(){
        WildcardGeneric<Car> wildcard = new WildcardGeneric<Car>();
        wildcard.setWildcard(new Car("Mustang"));
        boundedWildcardMethod(wildcard);
    }
    public void boundedWildcardMethod(WildcardGeneric<? extends Car> c){
        Car value = c.getWildcard();
        System.out.println(value);
    }
}

//출력값
//Car name=Mustang
```


# 제네릭 메소드
- 메소드 선언시 리턴 타입 앞에 제네릭한 타입을 선언해 주고, 그 타입을 매개 변수에서 사용하면 컴파일시 문제가 없고, 값도 할당할 수 있다.
```java
public class GenericWildcardSample {
    public static void main(String[] args) {
        GenericWildcardSample sample = new GenericWildcardSample();
        callGenericMethod();
    }
    public <T> void genericMethod(WildcardGeneric<T> c, T addValue) {
        c.setWildcard(addValue);
        T value = c.getWildcard();
        System.out.println(value);
    }
    public void callGenericMethod() {
        WildcardGeneric<String> wildcard = new WildcardGeneric<String>();
        genericMethod(wildcard,"Data");
    }
}

//출력값
//Data
```
다음과 같은 구조도 사용 가능하다.
```java
public void boundedGenericMethod(WildcardGeneric C,T addValue)
public <S,T extends Car> void multiGenericMethod(WildcardGeneric c,T addValue,S another)
```