# 패키지(package)
- 패키지는 클래스를 체계적으로 관리하기 위한 도구로 클래스들을 구분 짓는 폴더이다.  
- 패키지의 물리적인 형태는 파일 시스템의 폴더이며 단순히 파일 시스템의 폴더 기능만 하는 것이 아니라 클래스의 일부분이다.  
- 클래스를 유일하게 만들어주는 식별자이다. 클래스 이름이 동일해도 패키지가 다르면 다른 클래스로 인식한다. 또한 패키지 내부에 패키지를 둘 수도 있다.
- 클래스를 체계적으로 관리하지 않으면 수십, 수백 개의 클래스 간의 관계가 뒤엉켜 복잡해질 수 있으므로 패키지를 사용한다.

### 패키지 이름 규칙
- 숫자로 시작하거나, ‘_’ 과 ‘$’를 제외한 특수 문자를 사용 금지
- java로 시작하는 패키지 금지(자바 표준 API에서만 사용)
- int, static 등 자바 예약어 금지 
- 모두 소문자로 작성하는 것이 관례
```
상위패키지.하위패키지.클래스
godOfJava.package.example
```

## import
같은 패키지에 속하는 클래스들은 아무런 조건 없이 다른 클래스를 사용할 수 있지만, 다른 패키지에 속하는 클래스를 사용하려면 두 가지 방법 중 하나를 선택해야 한다.
1. 패키지와 클래스를 모두 기술(이러한 표현을 FQCN(Fully Qualified Class Name)이라고 함)
2. import 문을 사용

1번의 경우 패키지 이름이 길거나, 사용해야 할 클래스 수가 많다면 코드가 난잡해 보이게 된다. 그래서 두 번째 방법인 import문을 주로 사용한다.
```java
import godOfJava.package.example; //패키지 선언과 클래스 선언 사이에 써준다.

public class Main {
    
    public static void main(String[] args){
        Example example = new Example();
    }
}
```
패키지의 포함된 다수의 클래스를 사용해야 한다면 클래스 이름을 생략하고 대신 *를 사용해서 import문을 한 번 작성한다.
```java
import godOfJava.package.*; //
```

## 빌트 인 패키지(Built in Package)
자바는 개발자들이 사용할 수 있도록 여러 패키지를 제공한다.   
이 중에서 자주 사용하는 패키지는 import를 하지 않아도 사용할 수 있다.   
예를 들어 java.lang 패키지는 import 없이 사용할 수 있다.

이러한 패키지를 빌트 인 패키지라고 한다. 다음은 java.lang의 String 클래스, System 클래스를 사용하는 예시다.
```java
// import java.lang.String; 생략
// import java.lang.System; 생략
public class BuiltInPackage {
    public static void main(String[] args){
        String message = "저는 빌트 인 패키지 내부에 있습니다.";
        System.out.println(message);
    }
}
```

## static import
일반적인 import와 다르게 메소드와 변수를 패키지, 클래스명 없이 접근 가능
```java
import  org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

public class staticImportTest {
@Test
void nonStaticImport(){
int one = 1;
Assertions.assertThat(one).isEqualTo(1);
}

    @Test
    void staticImport(){
        int one = 1;
        assertThat(one).isEqualTo(1);
    }
}
```

위 코드는 변수 'one'이 정수 1이 맞는지 검사하는 단위 테스트다. 그 중에서 nonStaticImport 메소드는 static import를 적용하였고, staticImport 메소드는 static import를 적용하지 않았다.  

장점: 'That(one) is 1' 과 같이 영어 문장 읽듯이 코드를 읽을 수 있어서, 코드의 의도를 한 눈에 볼 수 있다는 점이다. 이러한 장점은 코드가 길어지고, 많은 메소드와 필드를 사용할 때 더 빛을 발한다.

클래스에 import한 동일한 이름의 static 변수나 static 메소드가 자신의 클래스에 있다면?
- import static으로 가져온 것보다 자신의 클래스에 있는 static 변수나 메소드가 우선된다.

