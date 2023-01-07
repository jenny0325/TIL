# Serializable interface
- 파일을 읽거나 쓰고, 다른 서버로 보내거나 받을 때 반드시 구현해야 한다.
- Serializable를 구현하면 JVM에서 해당 객체를 저장하거나 다른 서버로 전송할 수 있게 해 준다.
- Serializable를 구현한 후 serialVersionUID 값을 지정해 주는 것을 권장하는데, 만약 별도로 지정하지 않으면 컴파일할 때 자동으로 생성된다
- serialVersionUID은 해당 객체가 같은지 다른지 확인할 수 있도록 한다.
- 클래스 이름이 같더라도 serialVersionUID가 다르면 다른 클래스라고 인식한다, 그리고 같은 serialVersionUID라고 할지라도, 변수의 개수나 타입 등이 다르면 다른 클래스로 인식한다.
- 데이터가 변경되면 serialVersionUID를 변경하는 습관을 가지는게 좋다.
```java
// 반드시 이 구조로 선언해야 한다. 값은 아무런 값이나 지정해 주면 된다.
// 해당 객체의 버전을 명시한다
static final long serialVersionUID = 1;
```
## transient 예약어
- 객체를 저장하거나, 다른 JVM으로 보낼 때, transient 예약어를 사용해 선언한 변수는 Serializable의 대상에서 제외된다.
- 보안상 중요한 변수나 꼭 저장할 필요가 없는 변수에 사용한다.
```java
transient private int bookOrder;
```
# NIO
- JDK 1.4부터 추가되었다.
- 속도 때문에 생겼다.
- Channel과 Buffer를 사용한다.
- Channel은 객체를 생성하여 read()나 write() 메소드를 불러준다.
```java
public void writeFile(String fileName, String data) throws Exception {
    FileChannel channel = new FileOutputStream(fileName).getChannel();
    byte[] byteData = data.getBytes();
    ByteBuffer buffer = ByteBuffer.wrap(byteData);
    channel.write(buffer);
    channel.close();
}

public void readFile(String fileName) throws Exception {
    FileChannel channel = new FileInputStream(fileName).getChannel();
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    channel.read(buffer);
    buffer.flip();
    while(buffer.hasRemaining()) {
    System.out.print((char)buffer.get());
    }
}
```
## Buffer 클래스
- java.io.Buffer 클래스를 확장해 사용한다
- 데이터를 주고 받는다
### 버퍼의 상태 및 속성을 확인하기 위한 메소드
```java
- int capacity() : 버퍼에 담을 수 있는 크기 리턴
- int limit() : 버퍼에서 읽거나 쓸 수 없는 첫 위치 리턴
- int position() : 현재 버퍼의 위치 리턴

0 <= position <= limit <= 크기(capacity)
```
### 위치를 변경하는 메소드
```java
- Buffer flip() : limit 값을 현재 position으로 지정한 후, position을 0으로 이동
- Buffer mark() : 현재 position을 mark
- Buffer reset() : 현재 position을 mark한 곳으로 이동
- Buffer rewind() : 현재 버퍼의 position을 0으로 이동
- Buffer remaining() : limit-position 계산 결과를 리턴
- boolean hasRemainig() : limit와 position값의 차이가 있을 경우 true 리턴
- Buffer clear() : 버퍼를 지우고 현재 position을 0으로 이동하며 limit값을 버퍼의 크기로 변경
```
