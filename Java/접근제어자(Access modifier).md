# 접근 제어자
클래스를 설계할 때 외부 클래스에서 접근할 수 있는 멤버와 접근할 수 없는 멤버로 구분해서 필드, 생성자, 메소드를 설계하는 것이 바람직하다. 그 이유로 다음과 같은 상황이 있을 수 있기 때문이다.

1. 특정 객체 생성을 막기 위한 생성자 호출 제한
2. 객체의 특정 데이터를 보호하기 위한 필드에 접근 제한
3. 특정 메소드 호출을 막기 위해

접근 제어자의 종류: public, protected, default(package-private), private

1. public: 누구나 접근 가능
```
public class Main(){
}
```

2. protected: 같은 패키지 or 상속받은 경우 접근 가능
```
protected class Main(){
}
```
3. default(package-private): 같은 패키지만 접근 가능
```
class Main(){
}
```
4. private: 해당 클래스 내에서만 접근 가능
```
private class Main(){
}
```

|접근 제어자|적용 대상|접근할 수 없는 클래스|  
|:---------:|:---------------------:|:---------------:|
|public|	클래스, 필드, 생성자, 메소드	|없음|
|protected|	필드, 생성자, 메소드	|자식클래스가 아닌 다른 패키지에 소속된 클래스|
|default|	클래스, 필드, 생성자, 메소드	|다른 패키지에 소속된 클래스|
|private|	필드, 생성자, 메소드	|모든 외부 클래스|



## 클래스의 접근 제한

클래스를 선언할 때 고려할 사항: 같은 패키지 내에서만 사용인지 다른 패키지에서도 사용하는지 여부   
클래스에 적용할 수 있는 접근 제어자: public과 default
```java
package exam.com.packge1; // A 클래스와 패키지 동일

//public 접근 제한 -  같은 패키지 사용 가능, 다른 패키지에서도 사용 가능
public Class B{
A a; // A 클래스 접근 가능 (필드로 선언할 수 있음)
}

class A{
}
package exam.com.packge2;// A 클래스와 패키지 다름
import exam.com.packge1.*;

public class C{
A a; //(x) A 클래스 접근 불가, 컴파일 에러
B b; // B 클래스 접근 가능
}
```
주의사항
A.java라는 소스 코드에 A라는 클래스를 제외한 다른 클래스가 public으로 선언되어 있으면 컴파일이 불가하다.
```java
A.java

public class A {
}
//컴파일 오류 발생
public class B{
}
```

## 생성자의 접근 제한

생성자에 적용할 수 있는 접근 제어자: public, protected, default, privated
```java
public class ClassName{

    //public 접근 제한
    public ClassName(){}
    
    //protected 접근 제한
    protected ClassName(){}
    
    //default 접근 제한
    default ClassName(){}
        
        //private 접근 제한
        private ClassName(){}
}
```
- public: 모든 패키지에서 아무런 제한 없이 생성자를 호출 가능
생성자가 public 접근 제한을 가진다면 클래스도 public 접근 제한을 가져야 한다.  
클래스가 default 접근 제한을 가지면 클래스 사용이 같은 패키지로 한정되므로 같은 패키지에서만 생성자를 호출할 수 있기 때문이다.
- protected: 같은 패키지에 속하는 클래스 or 다른 패키지에 속한 클래스가 해당 클래스의 자식 클래스라면 생성자를 호출 가능
- default: 같은 패키지에서만 생성자를 호출 가능
- private:  생성자를 호출하지 못하도록 제한   
클래스 외부에서 new 연산자로 객체를 만들 수 없다.   
오로지 클래스 내부에서만 생성자를 호출하여 객체를 만들 수 있다.  
프로그램에서 클래스의 객체를 단 하나만 갖는 싱글톤 패턴 등에 사용될 수 있다.
```java
package 접근제어자.package1;

public class A{
//필드
A a1 = new A(true); //생성 ㅇ
A a2 = new A(1); //생성 ㅇ
A a3 = new A(1.0); //생성 ㅇ

    //생성자
    public A(boolean b) {} //public 접근 제한
    A(int b) {}; //default 접근 제한
    private A(double d) {} //private 접근 제한
}
```
```java
package 접근제어자.package1; // A 클래스와 패키지 동일

//public 접근 제한 -  같은 패키지 사용 가능, 다른 패키지에서도 사용 가능
public class B{
A a1 = new A(true); // 생성 ㅇ
A a2 = new A(1);    // 생성 ㅇ
A a3 = new A(1.0); // 생성 x, private 생성자 접근 불가 - 컴파일 에러

}
```
```java
package 접근제어자.package2;

import 접근제어자.package1.A;

public class C{
A a1 = new A(true); // 생성 ㅇ
A a2 = new A(1);     // 생성 x, default 생성자 접근 불가 - 컴파일 에어
A a3 = new A(1.0);  // 생성 x, private 생성자 접근 불가 - 컴파일 에러

}
```
## 필드와 메소드의 접근 제한
생성자에 적용할 수 있는 접근 제어자: public, protected, default, private
```java
package 접근제어자.package1;

public class A{
public int field1;
int field2;
private int field3;

    public void method1(){}
    void method2(){}
    private void method3(){}

}
```
```java
package 접근제어자.package1;

public class B{
public B(){
A a = new A();
a.field1 = 1; // 접근가능 o
a.field2 = 1; // 접근가능 o
a.field3 = 1; // 접근불가 x - private 필드 접근 불가 (컴파일 에러)

        a.method1();// 접근가능 o
        a.method2();// 접근가능 o
        a.method3();// 접근불가 x - private 필드 접근 불가 (컴파일 에러)
    }
}
```
```java
package 접근제어자.package2;

import 접근제어자.package1.A;

public class C {

    public C(){
        A a = new A();
        a.field1 = 1; // 접근가능 o
        a.field2 = 1; // 접근불가 x - default 필드 접근 불가 (컴파일 에러)
        a.field3 = 1; // 접근불가 x - private 필드 접근 불가 (컴파일 에러)
        
        a.method1();// 접근가능 o
        a.method2();// 접근불가 x - default 필드 접근 불가 (컴파일 에러)
        a.method3();// 접근불가 x - private 필드 접근 불가 (컴파일 에러)
    }
}
```