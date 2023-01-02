# Nested 클래스
- 코드를 간단하게 표현하기 위해 존재
- 자바 기반의 UI 처리를 할 때 사용자의 입력이나, 외부의 이벤트에 대한 처리를 하는 곳에서 많이 사용된다.
- 한 곳에서만 사용되는 클래스를 논리적으로 묶어서 처리할 필요가 있을 때 → Static Nested 클래스 사용
- 캡슐화가 필요할 때. 즉, 내부 구현을 감추고 싶을 때 → inner 클래스 사용  
예를 들어 A라는 클래스에 private 변수가 있다. 이 변수에 접근하고 싶은 B라는 클래스를 선언하고, B 클래스를 외부에 노출시키고 싶지 않을 경우
- 소스의 가독성과 유지보수성을 높이고 싶을 때 사용
- Static nested 클래스, inner 클래스 차이는 static 여부이다.

## Static nested 클래스
```java
public class OuterOfStatic {
    static class StaticNested {
        private int value = 0;
        public int getValue() {
        return value;
        }
        public void setValue(int value) {
            this.value=value;
        }
    }
}
```
```java
public class NestedSample {
    public static void main(String[] args){
        NestedSample sample = new NestedSample();
        sample.makeStaticNestedObject();
    }
    public void makeStaticNestedObject() {
        OuterOfStatic.StaticNested staticNested = new OuterOfStatic.StaticNested();
        staticNested.setValue(3);
        System.out.println(staticNested.getValue());
    }
}
```
Static Nested 클래스를 만들었을 때 객체 생성은 감싸고 있는 클래스 이름 뒤에 .을 찍고 쓰면된다.
## inner 클래스
### Local inner class
- 이름이 있는 클래스
- 하나의 클래스에서 어떤 공통적인 작업을 수행하는 클래스가 필요한데 다른 클래스에서는 그 클래스가 전혀 필요가 없을 때 내부 클래스를 만들어 사용한다.
```java
public class OuterOfInner {
    class Inner {
        private int value = 0;
        public int getValue() {
            return value;
        }
        public void setValue(int value) {
            this.value=value;
        }
    }
}
```
```java
public class InnerSample {
    public static void main(String[] args) {
        InnerSample sample = new InnerSample();
        sample.makeInnerObject();
    }
    public void makeInnerObject() {
        OuterOfInner outer = new OuterOfInner();
        OuterOfInner.Inner inner = outer.newInner();
        inner.setValue(3);
        System.out.println(inner.getValue());
    }
}
```
Inner클래스의 객체를 만들기 위해서 먼저 Inner 클래스를 감싸고 있는 클래스의 객체를 만들어야한다.
### Anonymous inner class
- 익명 클래스 (이름이 없는 클래스)
- 클래스에는 이름이 없지만 메소드가 구현되어 있다.
- 클래스를 만들고 호출하면 그 정보는 메모리에 올라간다. 즉, 클래스를 만들수록 메모리는 많이 필요해지고, 애플리케이션이 시작할 때 더 많은 시간이 소요된다.
따라서 자바에서는 이렇게 간단한 방법으로 객체를 생성할 수 있도록 해놓았다.
- 익명클래스나 내부 클래스는 다른 클래스에서 재사용할 일이 없을 떄 만들어야 한다.
- 익명 클래스로 코드의 가독성이 높아질 수도 있지만, 그 반대의 경우도 생길 수 있기 때문에 너무 남용하지는 말자
```java
public interface EventListener {
public void onClick();
}
```
```java
public class MagicButton {
private EventListener listener;

        public MagicButton(){}
        
        public void setListener(EventListener listener){
            this.listener = listener;
        }
        public void onClickProcess(){
            if(listener != null) {
                listener.onClick();
            }
        }
}
```
```java
public class AnonymousSample{
    public static void main(String[] args) {
        AnonymousSample sample = new AnonymousSample();
        sample.setButtonListener();
    }  

    public void setButtonListener() {
        MagicButton button = new MagicButton();
        MagicButtonListener listener = new MagicButtonListener();
        button.setListener(listener);
        button.onClickProcess();
    }
        
    public void setButtonListenerAnonymous() { // 익명 클래스
        MagicButton button = new MagicButton();
        button.setListener(new EventListener(){ public void onClick() {
            System.out.println("Magic Button Clicked !!!");
        }});
        button.onClickProcess();
       	}
    
    public void setButtonListenerAnonymousObject(){// 재사용할 수 있는 익명 클래스 
        MagicButton button = new MagicButton();
        EventListener listener = new EventListener(){
            public void onClick(){
                System.out.println("Magic Button Clicked !!!");
            } 
        };
        button.setListener(listener);
        button.onClickProcess();
    }

    class MagicButtonListener implements EventListener{
        public void onClick(){
            System.out.prinln("Magic Button Click !!!");
        }
    }
}
```
## Nested 클래스의 특징

### Static Nested 클래스
- 감싸고 있는 클래스의 static 변수만 참조 가능
### 내부 클래스, 익명 클래스
- 감싸고 있는 클래스의 어떤 변수라도 참조할 수 있다.
- 감싸고 있는 클래스는 Static Nested 클래스의 인스턴스 변수나 내부 클래스의 인스턴스 변수로 접근 가능하다
