# 자바의 역사
- 자바의 표준 버전 이름은  
JDK (Java Development Kit)  
J2SE (Java 2 Standard Edition)  
Java SE (Java Standard Edition)  
순으로 변경되어 왔다
- Java SE 6 까지는 Sun Microsystem라는 회사에서 자바에 대한 주요 스펙을 만들고, Java SE 7 부터는 Oracle이라는 회사가 Sun Microsystem 회사를 인수하여 지금에 이르고 있다.
- JDK (Java Development Kit)
- JRE (Java Runtime Environment) : 자바를 실행할 수 있는 환경의 집합, 
JRE만 설치하면 자바를 컴파일하는 등의 각종 프로그램이 제외된 상태로 설치된다.

![images_always_post_a6addb77-945e-47e2-b687-a099df56dda0_image](https://user-images.githubusercontent.com/89081374/210375766-7bd12e64-0b57-42e2-a971-7615f20f1f24.png)
각각의 블록은 자바에서 제공하는 라이브러리들이다.
# 자바의 특징
자바 개발 초기에 정해진 것으로, 약간은 지금과 다른 부분이 있다.  
하지만, 근간은 달라지지 않고 유지되고 있으므로 알아두면 좋다.

### it should be "Simple, object-oriented an familier" 
  - 단순하고, 객체 지향이며 친숙해야한다
### it should be "robust and secure"
  - 견고하며, 보안상 안전하다
  - 컴파일할 떄와 실행할 때 문법적 오류를 체크 한다.
###it should be "architecture-neutral and portable"
  - 아키텍처에 중립적이어야 하며 포터블해야 한다
  - 다양한 HW 아키텍처에서 수행할 수 있도록 되어 있다. 따라서 자바는 아키텍처에 중립적인 바이트 코드를 생성한다.
  - 아키텍처 중립적이라는 말은 포터블한 시스템의 일부분일 뿐이다. 기본 데이터 타입의 크기를 지정해 놓고, 숫자 연산자에 대한 행위들을 정의해 두었다. 따라서 어떤 플렛폼에서도 동일한 결과가 나오며 호환성 문제가 발생하지 않는다. 이것은 JVM 덕분이다
### it should execute with "high performance"
  - 높은 성능을 제공해야 한다
### it should be "interpreted, threaded, and dynamic"
  - 인터프리트 언어이며, 쓰레드를 제공하고, 동적인 언어이다.
  - 자바 인터프리터는 자바 바이트 코드를 어떤 장비에서도 수행할 수 있도록 해준다.
  - 멀티 쓰레드 환경을 제공하기 때문에, 동시에 여러 작업을 수행할 수 있다. 따라서 사용자에서 매우 빠른 사용 환경을 제공한다.
  - 자바 컴파일러는 컴파일시 매우 엄격한 정적인 점검을 수행한다. 그리고, 실행시에 동적으로 필요한 프로그램들을 링크시킨다.
# 자바 버전별 차이
### JDK 1.0
- 최초의 버전
- AWT의 이벤트 모델의 확장 및 변경
- 내부 클래스 추가
- JavaBeans, JDBC, RMI 등 추가
- RMI(Remote Method Invocation) : 원격 JVM에 있는 메소드를 호출하기 위한 기술
### JDK 1.2
- JDK 1.2~1.5버전까지 J2SE라고 불렀다.
- strictfp 예약어 추가
- Swing이 코어 라이브러리에 추가
- JIT라는 컴파일러가 Sun JVM에 처음 추가
- JIT(Just-In-Time):어떤 메소드의 일부 혹은 전체 코드를 네이티브 코드로 변환하여 JVM에서 번역하지 않도록 함으로써 보다 빠른 성능을 제공하는 기술
- 자바 플러그인 추가
- CORBA라는 기술과 데이터를 주고 받기 위한 IDL 추가 (지금은 별로 사용 X)
- Collections 프레임워크 추가
### JDK 1.3
- HotSpot JVM추가
- CORBA와의 호환성을 위해서 RMI 수정
- 사운드를 처리하기 위한 JavaSound 라이브러리 추가
- JNDI(Java Naming and Directory Interface) 코어 라이브러리에 추가
- 어떤 객체를 쉽게 찾을 수 있도록 도와주는 이름을 지정한 후, 그 이름으로 객체를 찾아가는 것
- 자바의 디버깅을 쉽게 하기 위한 JPDA(Java Platform Debugger Architecture) 추가
- Synthetic 프록시 클래스 추가
### JDK 1.4
- assert 예약어 추가
- Perl 언어의 정규 표현식을 따르는 정규 표현식 추가
- 정규표현식 : 어떤 문자열에서 특정 조건이 맞는 값이 있는지 확인
- exception chaining이라는 것을 통해 하위 레벨의 예외의 캡슐화가 가능해짐
- IPv6 지원 시작
- NIO(New Input/Output)라는 non-blocking 추가
- 각종 로그를 처리하기 위한 logging API 추가
- JPEG이나 PNG와 같은 이미지를 읽고 쓰기 위한 image I/O API추가
- Java Web Start 추가
- 각종 설정 값들을 저장하고 읽는 데 사용되는 Preference API(java.util,prefs) 추가
### JAVA 5
- 보다 안전하게 컬렉션 데이터를 처리할 수 있는 제네릭 추가
- 어노테이션이라고 불리는 메타데이터 기능 추가
- 기본 자료형과 그 기본 자료형을 객체로 다루는 클래스 간의 데이터 변환이 자동으로 발생하는 autoboxing 및 unboxing 기능 추가 (int와 Integer데이터의 자동 변환)
- 상수 타입을 나타내는 enum 추가
- 매개 변수를의 개수를 가변적으로 선언할 수 있는 vararges추가 (String ... str)
- for루프에 콜론으로 구분하여 배열이나 컬렉션 타입에 저장되어 있는 데이터 순차적으로 꺼내는 향상된 for루프 추가
- static import 추가
- 쓰레드 처리를 쉽게 할 수 있는 concurrent 패키지 추가
- 스트림이나 버퍼로 들어오는 데이터의 분석을 보다 간편하게 할 수 있는 Scanner 클래스 추가
### JAVA 6
- JAVA 5에 비해 크게 바뀌지 않았다. 안정성과 확장성이 높아졌다.
- 스크립팅 언어가 JVM 위에서 수행 가능하게 됨
- 각종 코어 기능의 성능 개선
- Compiler API가 추가되어 프로그램에서 자바 컴파일러 실행이 가능

# JIT
- Just-In-Time = 동적 변환
- 프로그램 실행을 보다 빠르게 함 실행시 적용되는 기술이다.
- 인터프리트 방식 + 정적 컴파일 방식
- 변환 작업은 인터프리터에 의해 지속적으로 수행되지만, 필요한 코드의 정보는 캐시에 담아두었다가(메모리에 올려두었다가) 재사용하게 된다.
- 컴퓨터 프로그램을 실행하는 방식
  - 인터프리트 방식 : 프로그램이 실행할 떄마다 컴퓨터 언어로 변환하는 작업 수행
  - 정적 컴파일 방식 : 실행하기 전에 컴퓨터가 알아 들을 수 있는 언어로 변환하는 작업을 미리 실행
- javac 명령어를 사용하면 텍스트java 파일을 어떤 OS에서도 수행될 수 있도록 바이트 코드로 만드는 것이다. 이를 컴퓨터가 이해할 수 있게 다시 변환 작업이 필요한데 이 변환 작업을 JIT 컴파일러에서 한다.  


  ![image](https://user-images.githubusercontent.com/89081374/210375461-540447fa-9a6d-4a02-9ca8-698a588d8e1b.png)
- JVM->기계코드 작업이 JIT에서 수행하는 것이다.
- 반복 수행 코드는 매우 빠른 성능을 보이지만 처음 시작할 때 변환 단계를 거쳐야 하므로 성능이 느리다는 단점이 있다.
# HotSpot
- HopSpot 클라이언트 컴파일러, HotSpot 서버 컴파일러 -> 두 가지 컴파일러 제공
- 옛날엔 CPU코어 개수가 하나 였는데 이런 사용자들을 위해 HotSpot 클라이언트 컴파일러가 만들어졌다. 애플리케이션 시작 시간을 빠르게 하고, 적은 메모리를 점유하도록 한다.
- 코어가 많은 장비에서 애플리케이션을 돌리기 위해 HotSpot 서버 컴파일러가 만들어졌다. 애플리케이션 수행속도에 초점이 맞춰져 있다.
- 자바가 시작할 떄 2개 이상의 물리적 프로세서를 가졌는지, 2GB 이상의 물리적 메모리를 가졌는지 판단해 클라이언트 장비인지 서버 장비인지를 확인한다. 혹은 OS 따라 정해져 있기도 하다.
# 자바 필수 용어
## JVM (Java Virtual Machine)
- 자바 프로그램이 수행되는 프로세스
- java라는 명령어를 통해서 애플리케이션이 수행되면, JVM 위에서 애플리케이션이 동작한다.
## GC (Garbage Collector)
- 자바의 메모리 관리는 JVM이 알아서 해주는데, JVM내에서 메모리 관리를 해주는 것을 가비지 컬렉터라고 한다.
- 사용하고 남아 있는 전혀 필요 없는 객체들이 속한다.
### GC 진행 방식
- Java7부터 공식적으로 사용할 수 있는 GI(Garbage First)라는 가비지 컬렉터를 제외한 나머지 JVM은 다음과 같이 영역을 나누어 힙이라는 공간에 객체들을 관리한다.
  
![images_always_post_d9997a5c-23fd-4a65-ade1-ac661797d50a_image](https://user-images.githubusercontent.com/89081374/210377325-57a43824-6030-4279-918e-b0aa38f66e5a.png)


- Young 영역은 젊은 객체들이 존재하며, Old 영역에는 늙은 객체들이 자리잡는다. 
- Perm 영역에는 클래스나 메소드에 대한 정보가 쌓인다.
- Young 영역은 Eden과 Survivor 영역으로 나뉘는데, 객체를 생성하자마자 저장되는 장소는 Eden이다.

### 메모리가 살아가는 과정 (minor GC or Young GC)

- Eden 영역에 객체가 생성된다
- Eden 영역이 꽉 차면 살아있는 객체만 Survivor 영역으로 복사되고, 다시 Eden 영역을 채우게 된다.
- Survivor 영역이 꽉 차게 되면 다른 Survivor 영역으로 객체가 복사된다. 이때, Eden 영역에 있는 객체들 중 살아있는 객체들도 다른 Survivor 영역으로 간다. 즉, Survivor영역의 둘 중 하나는 반드시 비어 있어야만 한다.
오래 살아있는 객체들은 Old 영역으로 이동한다.
- 지속적으로 이동하다가 Old영역이 꽉 차면 GC가 발생하는데 이것을 major GC, Full GC 라고 한다.
- Young GC가 Full GC보다 빠르다. 일반적으로 더 작은 공간이 할당되고, 객체들을 처리하는 방식도 다르기 때문이다.

### 오라클 JDK의 GC 방식

- Serial GC
  - 클라이언트용 장비에 최적화된 GC이기 때문에 WAS로 사용하는 JVM에서 사용하면 안된다.
- Parallel Young Generation Collector
- Parallel Old Generation Collector
- Concurrent Mark & Sweep Collector (줄여서 CMS)
- G1 (Garbage First)