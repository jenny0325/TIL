# Thread
- java 명령어를 사용해 자바 프로그램을 시작하면 적어도 하나의 JVM이 시작된다. 보통 JVM이 시작되면 자바 프로세스가 시작되고, main()메소드가 수행되면서 하나의 쓰레드가 시작된다.
- 하나의 프로세스 내에는 여러 쓰레드가 수행될 수 있다.
- 쓰레드가 만들어진 이유 : 프로세스가 하나 시작하려면 많은 자원이 필요하다. 여러 개의 프로세스를 띄워서 실행하면 각각 메모리를 할당해 주어야 한다. JVM은 기본적으로 옵션 없이 실행하면 적어도 32MB~64MB의 물리 메모리를 점유한다. 그에 반해, 쓰레드는 하나 추가하면 1MB 이내의 메모리를 점유한다. 그래서 쓰레드를 경량 프로세스라고도 부른다.
- 쓰레드가 수행되는, 우리가 구현하는 메소드는 run() 메소드다.
- 쓰레드를 시작하는 메소드는 start()이다.
- 쓰레드 클래스가 다른 클래스를 확장할 필요가 있을 경우에는 Runnable 인터페이스를 구현하면 되며, 그렇지 않은 경우에는 쓰레드 클래스를 사용하는 것이 편하다.
- start() 메소드를 호출하면, 쓰레드 클래스에 있는 run() 메소드의 내용이 끝나든, 끝나지 않든 간에 쓰레드를 시작한 메소드에서는 그 다음 줄에 있는 코드를 실행한다
# 쓰레드를 생성하는 방법
## Runable 인터페이스
- java.lang 패키지
- void run() : 쓰레드가 시작되면 수행되는 메소드
```java
public class RunnableSample implements Runable {
    public void run(){
        System.out.println("run method.");
    }
}

public class RunThreads{
    public static void main(String[] args) {
        RunThreads threads = new RunThreads();
        threads.runBasic();
    }
    public void runBasic() {
        RunableSample runnable = new RunableSample();
        new Thread(runnable).start();
    }
}
```
## Thread 클래스
- java.lang 패키지
- Runable 인터페이스를 구현한 클래스
- 쓰레드에 이름을 지정하지 않으면, 그 쓰레드의 이름은 "Thread-n"이다.(n은 쓰레드가 생성된 순서에 따라 증가)
- ThreadGroup : 쓰레드 그룹, ThreadGroup클래스 메소드를 사용할 수 있다.
```java
public class ThreadSample extends Thread {
    public void run(){
        System.out.println("run method");
    }
}

public class RunThreads {
    public static void main(String[] args) {
        RunThreads threads = new RunThreads();
        threads.runBasic();
    }
    public void runBasic() {
        ThreadSample thread = new ThreadSample();
        thread.start();
    }
}
```
### Thread 클래스 생성자
```java
- Thread() : 새로운 쓰레드를 생성한다.
- Thread(Runnable target) : 매개 변수로 받은 target 객체의 run()메소드를 수행하는 쓰레드를 생성
- Thread(Runnable target, String name) : 매개 변수로 받은 target 객체의 run() 메소드를 수행하고, name이라는 이름을 갖는 쓰레드를 생성
- Thread(String name): name이라는 이름을 갖는 쓰레드를 생성
- Thread(ThreadGroup group, Runnable target) : 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run() 메소드를 수행하는 쓰레드를 생성
- Thread(ThreadGroup group, Runnable target, String name) : 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run() 메소드를 수행하고, name이라는 이름의 쓰레드 생성
- Thread(ThreadGroup group, Runnable target, String name, long stackSize) : 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run()메소드를 수행하고, name이라는 이름을 갖는 쓰레드를 생성한다. 해당 쓰레드 스택의 크기는 stackSize만큼 가능하다.
- Thread(ThreadGroup group, String name) : 매개 변수로 받은 group의 쓰레드 그룹에 속하는 name이라는 이름을 갖는 쓰레드를 생성
```
### Thread 주요 메소드
```java
- void run() : 구현해야 하는 메소드
- long getId() : 쓰레드의 고유 id를 리턴한다. JVM에서 자동으로 생성해준다
- String getName() : 쓰레드 이름 리턴
- void setName() : 쓰레드 이름 지정
- int getPriority() : 쓰레드의 우선 순위 확인
- int setPriority  : 쓰레드의 우선 순위 지정
- boolean isDaemon() : 쓰레드가 데몬인지 확인
- void setDaemon(boolean on) : 쓰레드를 데몬으로 설정할지 말지 설정, 쓰레드를 시작(start())하기 전에 사용해야 데몬 쓰레드로 인식된다.
- StackTraceElement[] getStackTrace() : 쓰레드의 스택 정보를 확인
- Thread.State getState() : 쓰레드의 상태 확인
- ThreadGroup getThreadGroup() : 쓰레드의 그룹을 확인
- static void sleep(long millis)
- static void sleep(long milis, int nanos) : 첫 번째 매개 변수로 넘어온 시간(1/1,1000초) + 두번째 매개 변수로 넘어온 시간 (1/1,000,000,000초)만큼 대기
                                             InterruptedException으로(혹은 상위 예외) try-catch문을 작성한다.
```
```
***** 우선 순위는 대기하고 있는 상황에서 더 먼저 수행할 수 있는 순위를 말한다.
      웬만하면 기본값으로 사용하는 것을 권장한다. 잘못했다간 장애로 연결될 수 있다.
      우선순위는 다음과 같은 상수를 이용하는 것을 권장한다.
      1. MAX_PRIORITY : 가장 높은 우선 순위이며, 값은 10이다.
      2. NORM_PRIORITY : 일반 쓰레드의 우선 순위이며, 값은 5이다.
      3. MIN_PRIORITY : 가장 낮은 우선 순위이며, 값은 1이다.
***** 데몬 쓰레드 : 사용자 쓰레드는 JVM이 해당 쓰레드가 끝날 때까지 기다린다.
      데몬 쓰레드는 수행되고 있든, 수행되지 않고 있든 상관없이 JVM이 끝날 수 있다.
      해당 쓰레드가 종료되지 않아도 다른 실행중인 일반 ㅡ레드가 없다면, 멈춰 버린다.
      부가적인 작업을 수행할 때 사용한다 ex) 모니터링 쓰레드
```

### 쓰레드를 통제하는 메소드
```java
- Thread.State getState() : 쓰레드의 상태를 확인
- void join() : 수행중인 쓰레드가 중지할 때까지 대기
- void join(long millis) : 매개변수에 지정된 시간만큼(1/1,000초) 대기
- void join(long millis, int nanos) : 첫 번째 매개 변수에 지정된 시간(1/1,000초)+두 번째 매개 변수에 지정된 시간(1/1,000,000,000초)만큼 대기
- void interrupt() : 수행중인 쓰레드에 중지 요청을 한다
```
``` text
***** Thread.State enum 클래스
      NEW : 쓰레드 객체는 생성되었지만, 아직 시작되지 않은 상태
      RUNNABLE : 쓰레드가 실행중인 상태
      BLOCKED : 쓰레드가 실행 중지 상태이며, 모니터 락이 풀리기를 기다리는 상태
      WAITING : 쓰레드가 대기중인 상태
      TIMED_WAITING : 특정 시간만큼 쓰레드가 대기중인 상태
      TERMINATION : 쓰레드가 종료된 상태
      
      public static으로 선언되어 있기 때문에 Thread.State.NEW와 같이 사용할 수 있다.
      어떤 쓰레드던지 NEW->상태->TERMINATED 라이프 사이클을 가진다.
```
### 쓰레드의 상태를 확인하는 메소드
```java
- void checkAccess() : 현재 수행중인 쓰레드가 해당 쓰레드를 수정할 수 있는 권한이 있는지 확인, 만약 권한이 없다면 SecurityException 예외 발생
- boolean isAlive() " 쓰레드가 살아 있는지 확인, 해당 쓰레드의 run() 메소드가 종료되었는지를 확인
- boolean isInterrupted() : run() 메소드가 정상적으로 종료되지 않고, interrupt() 메소드의 호출을 통해 종료되었는지 확인, 다른 쓰레드에서 확인할 때 사용
- static boolean interrupted() : 현재 쓰레드가 중지되었는지를 확인
- static int activeCount() : 현재 쓰레드가 속한 쓰레드 그룹의 쓰레드 중 살아 있는 쓰레드의 개수를 리턴
- static Thread currentThread() : 현재 수행중인 쓰레드의 객체를 리턴
- static void dumpStack() : 콘솔 창에 현재 쓰레드의 스택 정보를 출력
```

### Obejct 클래스에 선언된 쓰레드와 관련있는 메소드들
```java
- void wait() : 다른 쓰레드가 Object 객체에 대한 notify() 메소드나 notifyAll() 메소드를 호출할 때까지 현재 쓰레드가 대기하고 있도록 한다.
- void wait(long timeout) : wait() 메소드와 동일한 기능 제공, 매개 변수에 지정한 시간만큼(1/1,000초) 대기
- void wait(long timeout, int nanos) : wait() 메소드와 동일한 기능 제공, 나노초(1/1,000,000,000초)를 이용해 더 자세히 설정 할 수 있다.
- void notify() : Object 객체의 모니터에 대기하고 있는 단일 쓰레드를 깨운다.
- void notifyAll() : Object 객체의 모니터에 대기학 있는 모든 쓰레드를 깨운다.
```
### ThreadGroup에서 제공하는 메소드들
- ThreadGroup은 기본적으로 운영체제의 폴더처럼 뻗어나가는 트리 구조를 가진다.
```java
- int activeCount() : 실행중인 쓰레드의 개수를 리턴
- int activeGroupCount() : 실행중인 쓰레드 그룹의 개수를 리턴
- int enumerate(Thread[] list) : 현재 쓰레드 그룹에 있는 모든 쓰레드를 매개 변수로 넘어온 쓰레드 배열에 담는다, 배열에 저장된 쓰레드의 개수를 리턴
- int enumerate(Thread[] list, boolean recurse) : 현재 쓰레드 그룹에 있는 모든 쓰레드를 매개 변수로 넘어온 쓰레드 배열에 담는다. 두 번째 매개 변수가 true이면 하위에 있는 쓰레드 그룹에 있는 쓰레드 목록도 포함한다.
                                                  배열에 저장된 쓰레드의 개수를 리턴
- int enumerate(ThreadGroup[] list) : 현재 쓰레드 그룹에 있는 모든 쓰레드 그룹을 매개 변수로 넘어온 쓰레드 그룹 배열에 담는다.
                                      배열에 저장된 쓰레드의 개수를 리턴
- int enumerate(ThreadGroup[] list, boolean recurse) : 현재 쓰레드 그룹에 있는 모든 쓰레드 그룹을 매개 변수로 넘어온 쓰레드 그룹 배열에 담는다. 두 번째 매개 변수가 true이면 하위에 있는 쓰레드 그룹 목록도 포함한다.
                                                       배열에 저장된 쓰레드의 개수를 리턴
- String getName() : 쓰레드 그룹의 이름을 리턴
- ThreadGroup getParent() : 부모 쓰레드 그룹을 리턴
- void list() : 쓰레드 그룹의 상세 정보를 출력
- void setDaemon(boolean daemon) : 지금 쓰레드 그룹에 속한 쓰레드들을 데몬으로 지정
```

# Synchronized
- 여러 쓰레드에서 하나의 객체에 있는 인스턴스 변수를 동시에 처리할 때 발생할 수 있는 문제를 해결하기 위해 사용하는 것
- 어떤 메소드나 클래스가 쓰레드에게 안전하려면, synchronized를 사용해야만 한다.
- 메소드에서 인스턴스 변수를 수정 할 때 여러 쓰레드가 동시에 연산을 수행하여 값이 꼬일 수 있는데 이럴 떄 synchronized를 사용한다.
- 두 개의 쓰레드가 같은 클래스에서 생성된 서로 다른 객체를 참조한다면 synchronized로 선언된 메소드는 같은 객체를 참조하는 것이 아니므로, synchronized를 안쓰는 것과 동일
```java
***** StringBuffer / StringBuilder
StringBuffer는 synchronized 블록으로 주요 데이터 처리하는 부분을 감싸 두었고
SringBuilder는 synchronized를 사용하지 않았다.
따라서, StringBuffer는 하나의 문자열 객체를 여러 쓰레드에서 공유해야 하는 경우에만 사용하고
StringBuilder는 여러 쓰레드에서 공유할 일이 없을 때 사용한다.
```

### 메소드를 synchronized로 선언
- 실행중인 synchronized 메소드를 다른 쓰레드에서 수행하려고 하면, 앞서 수행되는 메소드가 끝날때까지 기다리게 된다.
```java
public synchronized void plus(int value){
    amount+=value;
}
```
### 메소드 내의 특정 문장만 synchronized로 감싸기
```java
public void plus(int value){
    synchronized(this) {
        amount+=value;
    }
}

혹은

Object lock = new Object();
    public void plus(int value) {
        synchronized(lock) {
            amount+=value;
    }
}
```
# 정리
프로세스와 쓰레드 차이 : 프로세스는 운영체제로부터 자원을 할당받는 작업의 단위이다. 쓰레드는 할당 받은 자원을 이용하는 실행 단위이고 프로세스 내에 여러 개가 생길 수 있다.