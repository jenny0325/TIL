# 컴파일(Compile)이란?
<hr/>
- 컴파일 : 사람이 이해할 수 있는 언어로 작성된 소스코드를 컴퓨터가 이해할 수 있는 언어로 바꿔주는 과정이다.<br/>
- 컴파일러 : 컴파일을 수행하는 소프트웨어이다.
<br/><br/>
<img width="711" alt="images_woo00oo_post_6b0b5cb5-6734-4fa1-8bb0-057dc1070cc4_스크린샷 2021-01-01 오후 6 14 46" src="https://user-images.githubusercontent.com/89081374/206717271-ab86e112-2a20-4deb-bb25-8ffbad0d2add.png">

자바는 OS에 독립적인 특징을 가지고 있다.   
그게 가능한 이유는 JVM(Java Vitual Machine) 덕분이다.  
### JVM이란?
Java Virtual Machine의 줄임말로 직역하면 '자바를 실행하기 위한 가상 기계(컴퓨터)'라고 할 수 있다.  
Java 는 OS에 종속적이지 않다는 특징을 가지고 있다. OS에 종속받지 않고 실행되기 위해선 OS 위에서 Java 를 실행시킬 무언가가 필요하다. 그게 바로 JVM이다.  
즉, OS에 종속받지 않고 CPU 가 Java를 인식, 실행할 수 있게 하는 가상 컴퓨터이다.  
그렇다면 JVM의 어떠한 기능 때문에, OS에 독립적으로 실행시킬 수 있는지 자바 컴파일 과정을 통해 알아보도록 하자.
  
## 자바 컴파일 과정
![991D064B5AE999D512](https://user-images.githubusercontent.com/89081374/206716235-3ed0f9d8-d854-487e-a8b5-51c46c1497fd.jpeg)
![img_java_programming](https://user-images.githubusercontent.com/89081374/206716279-e974cffe-2348-4cd6-bdc6-22c0432aa176.png)
1. 개발자가 자바 소스코드(.java)를 작성합니다. 
2. 자바 컴파일러(Java Compiler)가 자바 소스파일을 컴파일합니다. 이때 나오는 파일은 자바 바이트 코드(.class)파일로 아직 컴퓨터가 읽을 수 없는 자바 가상 머신이 이해할 수 있는 코드입니다. 바이트 코드의 각 명령어는 1바이트 크기의 Opcode(컴퓨터에서의 연산(operation) 종류를 나타내기 위한 코드)와 추가 피연산자로 이루어져 있습니다. 
3. 컴파일된 바이크 코드를 JVM의 클래스로더(Class Loader)에게 전달합니다. 
4. 클래스 로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올립니다.

### 클래스 로더 세부 동작
- 로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드합니다. 
- 검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사합니다. 
- 준비 : 클래스가 필요로 하는 메모리를 할당합니다. (필드, 메서드, 인터페이스 등등)
- 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경합니다. 
- 초기화 : 클래스 변수들을 적절한 값으로 초기화합니다. (static 필드)
  
5. 실행엔진(Execution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행합니다. 이때, 실행 엔진은 두가지 방식으로 변경합니다. 
   - 인터프리터 : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행합니다. 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가집니다.
   - JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식입니다. 하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠릅니다.
<br/>
<br/>
<br/>
<br/>
출처 : [Tech Interview](https://gyoogle.dev/blog/computer-language/Java/%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.html)