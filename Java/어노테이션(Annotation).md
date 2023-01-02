# 어노테이션
- Annotation = Metadata
- java.lang.annotaion 패키지에 선언되어 있다.
- 클래스나 메소드 등 선언시에 @를 사용하는 것
- JDK5부터 등장
- 컴파일러에게 정보를 알려줌
- 컴파일할 때와 설치(deployment)시의 작업을 지정
- 실행할 때 별도의 처리가 필요할 때 사용
- 자바 언어에는 사용하기 위해서 정해져 있는 어노테이션은 3개가 있고, 어노테이션을 선언하기 위한 메타 어노테이션이라는 것은 4개가 있다.

## 일반적으로 사용 가능한 어노테이션

### @Override
- 해당 메소드가 부모 클래스에 있는 메소드를 Override 했다는 것을 명시적으로 선언
- 자식클래스에서 Override한 메소드를 쉽게 알 수 있다

### @Deprecated
- 클래스나 메소드가 더 이상 사용되지 않는 경우

### @Deprecated 표기된 메소드를 사용하면 컴파일 시 경고 문구가 뜬다
- 안쓴다고 지워버렸는데 다른 개발자가 참조하고 있으면 컴파일 에러가 발생한다. 그러므로, 하위 호환성을 위해 Deprecated로 선언한다.

### @SupressWarnings
- 컴파일러가 경고를 알리는 경우가 있는데 이를 무시한다.

## 메타 어노테이션
어노테이션을 선언할 떄 사용

### @Target
- 어노테이션을 어떤 것에 적용할지를 선언할 때 사용
- @Target()괄호 안에 적용 대상을 지정

### @Retention
- 얼마나 오래 어노테이션 정보가 유지되는지 선언
- 괄호 안에 적용 대상 지정

### @Documented
- 해당 어노테이션에 대한 정보가 Javadocs(API) 문서에 포함된다는 것을 선언

### @Inherited
- 모든 자식 클래스에서 부모 클래스의 어노테이션을 사용 가능하다는 것을 선언

### @interface
- 어노테이션을 선언할 때 사용한다.

```java
@Targer(ElementType.METHOD) // 이 어노테이션은 메소드에 사용할 수 있다고 지정
@Retention(RetentionPolicy.RUNTIME) // 실행시에 이 어노테이션을 참조
public @interface UserAnnotation{ // @UserAnnotation으로 어노테이션이 사용 가능
    public int number();  // 메소드처럼 어노테이션 안에 선언해 놓으면, 이 어노테이션을 사용할 때 해당 항목에 대한 타입으로 값을 지정 가능하다.
    public String text() default "This is first annotation"; // default 뒤에 있는 값이 이 어노테이션을 사용할 때의 기본값이 된다.
}
```
```java
public class UserAnnotationSample{
    @UserAnnotation(number=0)
    public static void main(String[] args) {
        UserAnnotationSample sample = new UserAnnotationSample();
    }

    @UserAnnotation(number=1)
    public void annotationSample1(){}
	
    @UserAnnotation(number=2, text="second")
    public void annotationSample2() {}

    @UserAnnotation(number=3, text="second")
    public void annotationSample3() {}
}
```

- 메소드를 선언할 때 직접 만든 어노테이션을 사용할 수 있다.  
어노테이션 선언 클래스에 지정해 놓은 각 메소드의 이름에 해당하는 값을 소괄호 안에 넣어 주어야만 한다.
- 어노테이션은 상속이 불가능하다.
- 어노테이션을 지정하면 코드가 내부적으로 어떻게 변환되는지에 대해서 살펴보는 습관을 가지는 것이 좋다.