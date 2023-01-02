<h1>1. 클래스가 뭔가요?</h1>  
클래스는 Java의 가장 작은 단위이며 상태와 행동을 가지고 있다.
클래스 안에 변수를 선언하면 이를 상태라 하고 메소드를 선언하면 행동이라고 볼 수 있다.


<h1>2. 메소드가 뭔가요?</h1>  

메소드는 클래스 내에서 행동에 속하는 부분으로 특정한 작업을 수행하는 단위이다. 매개변수로 값을 받을 수 있고 값을 받지 않아도 된다.  
메소드 내에 작성된 코드에 따라 특정한 행동을 수행한 후 리턴타입에 따라 값을 반환하기도 하고 리턴타입이 void인 경우에는 작업만 수행하고 행동을 마친다.  
메소드는 클래스에 소속되어야 하며 접근제어자 리턴타입 메소드이름(매개변수){}의 형식에 따라 만들 수 있다.

<h1>3. 메소드의 매개 변수(parameter)는 어디에 적어주나요?</h1>

메소드의 이름 옆의 소괄호 안에 매개 변수를 적어 준다.  
```java
public class Calculator {
    //접근제어자 리턴타입 메소드이름(매개변수){}
    public int add(int num1, int num2) {
        int sum;
        sum = num1 + num2;
        return sum;
    }
}
```
<h1>4. 메소드 이름 앞에 꼭 적어줘야 하는 건 뭐죠?</h1>

접근 제어자. 접근 제어자에는 public, protected, default, pricvate 등이 있고 각각의 접근 제어자는 모두 다른 접근 권한을 가지고 있다.  
- public : 모든 곳에서 접근 가능  
- protected : 같은 패키지에 있는 클래스와 상속 관계에 있는 클래스만 접근 가능  
- default : 같은 패키지에 있는 클래스만 접근 가능  
- private : 현재 클래스에서만 접근 가능  

<h1>5. 클래스가 갖고 있어야 한다고 한 두 가지가 뭐죠?</h1>

상태(state)와 행동(behavior).

<h1>6. 메소드에서 결과를 돌려주려면 어떤 예약어를 사용해야 하나요?</h1>

리턴 타입에 void가 아닌 리턴 타입을 지정해주고 return 예약어를 통해 결과를 반환할 수 있다.  
`*`예약어란?: 컴퓨터 프로그래밍 언어에서 이미 문법적인 용도로 사용되고 있기 때문에 식별자로 사용할 수 없는 단어들이다(클래스, 매소드, 변수 이름으로 사용 x)

출처 : 자바의신 