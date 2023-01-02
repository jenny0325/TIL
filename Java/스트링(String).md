# String 

## 클래스 선언
```Java
public final class String extends Object implements Serializable, Comparable, CharSequence
```

### Serializable 
구현해야 하는 메소드가 하나도 없는 특별한 인터페이스다.
Serializable를 implements하면 해당 객체를 파일로 저장하거나 다른 서버에 전송 가능한 상태가 된다.

### Comparable
compareTo()라는 메소드 하나만 선언되어 있다.
매개 변수로 넘어가는 객체와 현재 객체가 같은지 비교 같으면0, 순서 상으로 앞에 있으면 -1, 뒤에 있으면 1을 리턴
객체의 순서를 처리할 떄 유용하게 사용될 수 있다.

### CharSequence
해당 클래스가 문자열을 다루기 위한 클래스 라는 것을 명시적으로 나타낸다.

하단에 있는 정리는 많이 사용하는 메소드 위주로 작성되었다.

## String 생성자

- String(byte[] bytes)
- String(byte[] bytes, Charset charset)  
  - 지정된 캐릭터 셋을 사용하여 제공된 byte배열을 디코딩한 String 객체를 생성한다.
  - charset : 문자의 집합. 특정 나라의 글자


## String 문자열을 byte로 변환하기
- getBytes(Charset charset)
  - 지정한 캐릭터 셋 객체 타입으로 바이트 배열을 생성
  - 현재의 문자열 값을 byte배열로 변환하는 메소드
  - 플랫폼의 기본 캐릭터 셋으로 변환
```java
public void convertUTF16() {
    try {
        String korean = "한글";
        byte[] array1 = korean.getBytes("UTF-16");
        printByteArray(array1);
        System.out.println(korean2);
    } catch(Exception e) {
        e.printStackTrace();
    }
}

public void printByteArray(byte[] array){
    for(byte data:array) {
        System.out.print(data+" ");
    }
    System.out.println();
}

//결과 
// -2 -1 -43 92 -82 0
// 한글 
```
- 캐릭터 셋을 지정하는 메소드 및 생성자들은 존재하지 않는 캐릭터 셋의 이름을지정하는 경우 UnsupportedEncodingException을 발생시킬 수 있다. 따라서 반드시 try-catch감싸주거나 메소드 선언시 throws 구문을 추가해준다.
- String 메소드를 사용하기 전 널 체크를 반드시 해야 한다. (다른 모든 객체도 마찬가지)

## String클래스 객체의 내용을 비교하고 검색하는 메소드
### 문자열의 길이를 확인하는 메소드
- length()
  - 참고: 배열의 크기를 확인할 떄는 괄호가 없는 length를 사용  

### 문자열이 비어 있는지 확인하는 메소드
- isEmpty()

### 문자열이 같은지 비교
- boolean equals(Object anObject)
- boolean equalsIgnoreCase(String anotherStr)
- int compareTo(String antherStr)

### 매개변수가 알파벳 순으로 앞에 있으면 양수를, 뒤에 있으면 음수를 리턴
- int compareToIgnoreCase(String str)
- boolean contentEquals(CharSequence cs)
- boolean contentEquals(Stringbuffer sb)  
  - 매개변수로 넘어오는 CharSequence 와 Stringbuffer 객체가 String 객체와 같은지 비교  
### 특정 조건에 맞는 문자열이 있는지 확인
- boolean startWith(String prefix)  
  - 매개 변수로 넘겨준 값으로 시작하는지 확인
- boolean startWith(String prefix, int toffset)
- boolean endsWith(String suffix)  
  - 매개 변수로 넘어온 값으로 해당 문자열이 끝나는지 확인
- boolean contains(CharSequence s)  
  - 매개 변수로 넘어온 값이 문자열에 존재하는지 확인
- boolean matches(String regex)
  - 매개 변수로 넘어온 값이 정규 표현식(공식에 따라 만든 식)으로 되어 있어야한다.
- boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)
  - 문자열 중에서 특정 영역이 매개 변수로 넘어온 문자열과 동일한지를 확인하는 데 사용
- boolean regionMatches(int toffset, String other, int ooffset, int len)
- ignoreCase : true일 경우 대소문자 구분을 하지 않고, 값을 비교한다.
- toffset : 비교 대상 문자열의 확인 시작 위치를 지정한다.
- other : 존재하는지를 확인할 문자열을 의미한다.
- ooffset : other 객체의 확인 시작 위치를 지정한다.
- len : 비교할 char의 개수를 지정한다.

### String 내에서 위치를 찾아내는 방법
- indexOf()
  - 앞에서부터 해당 객체의 특정 문자열이나 char가 있는 위치를 알 수 있다. 없으면 -1을 리턴한다.
- int indexOf(int ch, int fromIndex)
- int indexOf(String str)
- int indexOf(String str, int fromIndex)
- int lastIndexOf(int ch)
  - 뒤에서부터 문자열이나 char 찾는다. 시작 위치는 가장 왼쪽에서 부터이다.
- int lastIndexOf(int ch, int fromIndex)
- int lastIndexOf(String str)
- int lastIndexOf(String str, int fromIndex)

## String의 값의 일부를 추출하기 위한 메소드들

### char 단위의 값을 추출하는 메소드
- char charAt(int index)
  - 특정 위치의 char값 리턴

### char 배열의 값을 String으로 변환
- static String copyValueOf(char[] data)
  - char 배열에 있는 값을 문자열로 변환한다.
- static String copyValueOf(char[] data, int offset, int count)
  - char배열에 있는 값을 offset 위치부터 count까지의 개수만큼만 문자열로 변환
  - cstatic 메소드이기 때문에 현재 사용하는 문자열을 참조하여 생성하는 것이 아닌, static하게 호출하여 사용해야 한다.
```java
char valuse[] = new char[]('a','b','c','d');
String javaText = String.copyValueOf(values);
```
### String의 값을 char 배열로 변환
chrar[] toCharArray()
### 문자열의 일부 값을 잘라내는 메소드
- String substring(int beginIndex)
  - beginIndex부터 끝까지 대상 문자열을 잘라 String으로 리턴
- String substring(int beginIndex, in endIndex)
  - beginIndex부터 endIndex까지 대상 문자열을 잘라 String으로 리턴
- CharSequence subSequence(int beginIndex, int endIndex)
  - beginIndex부터 endIndex까지 대상 문자열을 잘라 CharSequence타입으로 리턴

### 문자열을 여러 개의 String배열로 나누는 메소드
- String[] spilit(String regex)
  - regex에 있는 정규 표현식에 맞추어 문자열을 잘라 String의 배열로 리턴
- String[] split(String regex, int Limit)
  - regex에 있는 정규 표현식에 맞추어 문자열을 잘라 String의 배열로 리턴. 이때 String 배열의 크기는 limit보다 커서는 안된다.

## String 값을 바꾸는 메소드

### 문자열을 합치는 메소드와 공백을 없애는 메소드
- String cocat(String str)
  - 매개 변수로 받은 str을 기존 문자열의 우측에 붙인 새로운 문자열 객체를 생성해 리턴
- String trim()
  - 문자열의 맨 앞과 맨 뒤에 있는 공백들을 제거한 무자열 객체 리턴

### 내용을 교체하는 메소드
- String repalce(char oldChar, char newChar)
  - 해당 문자열에 있는 oldChar의 값을 newChar로 대치
- String replace(CharSequence target, CharSequence replacement)
  - 해당 문자열에 있는 target과 같은 값을 replacement로 대치
- String replaceAll(String regex, String replacement)
  - 해당 문자열의 내용 중 regex에 표현된 정규 표현식에 포함되는 모든 내용을 replacement로 대치
- String replaceFirst(String regex, String replacement)
  - 해당 문자열의 내용 중 regex에 표현된 정규 표현식에 포함되는 첫번째 내용을 replacement로 대치
  - 특정 형식에 맞춰 값을 치환하는 메소드
- staitc String format(String format, Object.. args)
  - format에 있는 문자열의 내용 중 변환해야 하는 부분을 args의 내용으로 변경
- static String format(Locale l, String format, Object... args)
  - format에 있는 문자열의 내용 중 변환해야 하는 부분을 args의 내용으로 변경
  - 단, 첫 매개 변수인 Locale타입의 l에 선언된 지역에 맞추어 출력

### 대소문자를 바꾸는 메소드
- String toLowerCase()
  - 모든 문자열의 내용을 소문자로 변경
- String toLowerCase(Locale locale)
  - 지정한 지역 정보에 맞추어 모든 문자열의 내용을 소문자로 변경
- String toUpperCase()
  - 모든 문자열의 내용을 대문자로 변경
- String toUpperCase(Locale locale)
  - 지정한 지역 정보에 맞추어 모든 문자열의 내용을 대문자로 변경

### 기본 자료형을 문자열로 변환하는 메소드
- static String valueOf(...)
  - 기본 자료형을 String타입으로 변환
  - 객체가 null이면 "null"이라는 문자열을 리턴
  
## ==, equals()메소드
- String 클래스에서 =="비교문자열", equals("비교문자열") 메소드가 동일한 결과가 나오는 이유는 자바에서는 객체들을 재사용하기 위해서 문자열 Pool이라는 것이 만들어져 있고, String의 경우 동일한 값을 갖는 객체가 있으면 이미 만든 객체를 사용
- new String("")으로 객체를 생성하면 값이 같은 String객체를 생성한다고 하더라도 Constant Pool의 값을 재활용하지 않고 별도의 객체를 생성한다.
```java
public void check(){
    String text1 = "java basic";
    String text2 = "java basic";
    String text3 = new String("java basic");
    System.out.println(text1 == text2);
    System.out.println(text2 == text3);
    System.out.println(text1.equals(text3));
}

    결과
    true
    false
    true

    [해설]
    text1, text2와 같이 객체를 생성하면, 
    String 클래스에서 관리하는 문자열 풀에 해당 값이 있으면 기존에 있는 객체를 참조하고, 
    text3와 같이 String 객체를 생성하면 같은 문자열이 풀에 있든 말든 새로운 객체를 생성한다.
```
equals()메소드로 비교하는 것보다 ==로 비교하는게 빠르다


## StringBuffer,StringBuilder
String은 한 번 만들어지면 더 이상 값을 바꿀수 없는 immutable한 객체다.
### StringBuffer
- Thread safe하다.
- StringBuilder보다 안전하다
- 문자열을 더해도 새로운 객체를 생성하지 않는다.
- append()메소드를 이용해 문자열을 추가
### StringBuilder
- Thread safe하지 않다
- StringBuffer보다 빠르다.
- 문자열을 더해도 새로운 객체를 생성하지 않는다.
- append()메소드를 이용해 문자열을 추가
- JDK5 이상에서는 String의 더하기 연산을 할 경우, 컴파일할 때 자동으로 해당 연산을 StringBuilder로 변환해준다. 따라서, 일일이 더하는 작업을 변환해 줄 필요는 없으나, for루프와 같이 반복 연산을 할 때에는 자동으로 변환 해주지 않으므로, 꼭 필요하다.

## String, StringBuilder, StringBuffer
- 공통점 : 모두 문자열을 다루고 CharSequence인터페이스를 구현했다.
- 세 가지 중 하나의 클래스를 사용하여 매개 변수로 받는 작업을 할 때 CharSequence타입으로 받는게 좋다.
- 일반적으로 하나의 메소드 내에서 문자열을 생성해 더할 경우 StringBuilder 사용
- 어떤 클래스에 문자열을 생성하여 더하기 위한 문자열을 처리하기 위해 인스턴스 변수가 선언되었고, 여러 쓰레드에서 이 변수를 동시에 접근하는 일이 있을 경우에는 반드시 StringBuffer를 사용해야만 한다.