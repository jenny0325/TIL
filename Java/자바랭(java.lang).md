# java.lang
- java.lang패키지에 있는 클래스들은 import를 안해도 사용할 수 있다
- 자바에서 꼭 필요한 여러 기능들을 제공한다
### java.lang 패키지 에러
- OutOfMemoryError(OOME)
  - 메모리 부족 에러. 자바는 가상 머신에서 메모리를 관리하지만, 프로그램을 잘못 작성하거나 설정이 제대로 되어 있지 않을 경우에는 발생할 수 있다.
- StackOverflowError
  - 호출된 메소드의 깊이가 너무 깊을 때 발생
  - 자바에서는 스택 영역에 어떤 메소드가 어떤 메소드를 호출했는지에 대한 정보를 관리한다. 예를들어 재귀 메소드를 잘못 작성했다면 스택에 쌓을 수 있는 메소드 호출 정보의 한계를 넘어설 수 있는데 이 경우에 발생한다
### 선언되어 있는 어노테이션
- @Deprecated
- @Override
- @SuppressWarnings

##숫자를 처리하는 클래스들
- 기본 자료형은 스택에 저장되어 관리된다.
- 기본 자료형으로 선언되어 있는 타입의 클래스들
- 겉보기엔 참조 자료형이지만, 기본 자료형처럼 사용할 수 있다.
- 자바 컴파일러에서 자동으로 형 변환을 해준다.
- 종류
  - Byte
  - Short
  - Integer
  - Long 
  - Float 
  - Double 
  - Character 
  - Boolean
- Character와 Boolean을 제외한 숫자를 처리하는 클래스들은 Wrapper클래스라고 불리며, 모두 Number라는 abstract 클래스를 확장한다.
- Character클래스를 제외하고는 공통적으로 parse타입이름() 메소드와 valueOf()메소드를 제공한다.
## System 클래스
- System 클래스의 3개의 static 변수
  ![images](https://user-images.githubusercontent.com/89081374/210378282-38d66a56-7832-48dc-a63a-6d6b8446857e.png)
- 시스템에 대한 정보를 확인하는 클래스
- 생성자가 없다.
- 속성값(Property), 환경값(Environment) 차이
  - 속성값은 변경이 가능하지만 환경값은 변경이 불가능하고 읽기만 할 수 있다.
- static long currentTimeMillis()
  - 밀리초 리턴. 현재 시간 확인을 위한 메소드
- static long nanoTime()
  - 나노초 리턴. 시간 측정을 위한 메소드
## System.out
- PrintStream의 클래스 객체
- print()
- println()
- format()
- printf()
- write()
```java
public void printNull(){
    Object obj = null;
    System.out.println(obj);
    System.out.println(obj+" is object's value);
}
// 출력 결과
// null
// null is object's value
```

print()메소드와 println()메소드에서는 단순히 toString()메소드 결과를 출력하지 않는다.  
String의 valueOf()라는 static메소드를 호출하여 결과를 받은 후 출력한다.  
객체를 출력할 때 toString()을 사용하기 보다 valueOf()메소드를 사용하는 것이 안전하다.