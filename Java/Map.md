# Map
- key와 value가 1:1로 저장된다
- 키 값은 해당 map에서 고유해야만 한다
## Map 인터페이스 주요 메소드
``` java
- V put(K key, V value)
    - 첫 번째 매개 변수인 키를 갖는, 두 번째 매개 변수인 값을 갖는 데이터를 저장
- void putAll(Map<? extends K, ? extends V> m)
    - 매개 변수로 넘어온 Map의 모든 데이터를 저장
- V get(Object key)
    - 매개 변수로 넘어온 키에 할당하는 값을 넘겨준다
- V remove(Object key)
    - 매개 변수로 넘어온 키에 해당하는 값을 넘겨주며, 해당 키와 값은 Map에서 삭제
- Set<K> keySet()
    - 키의 목록을 Set타입으로 리턴
- Collection<V> values()
    - 값의 목록을 Collection 타입으로 리턴
- Set<Map.Entry<K,V>> entrySet()
    - Map 안에 Entry라는 타입의 Set을 리턴
- int size()
    - Map의 크기를 리턴
- void clear()
    - Map의 내용을 지운다.
 ```
## Map을 구현한 주요 클래스
- HashMap
- TreeMap
- LinkedHashMap
- Hashtable
  - Map은 컬렉션 뷰를 사용하지만, Hashtable은 Enumeration객체를 통해 데이터를 처리한다.
  - Map은 키, 값, 키-값 쌍으로 데이터를 순환하여 처리할 수 있지만 Hashtable은 이중에서 키-값 쌍으로 데이터를 순환하여 처리할 수 없다
  - Map은 이터레이션을 처리하는 도중에 데이터를 삭제하는 안전한 방법을 제공하지만, Hashtable은 그러한 기능을 제공하지 않는다.
- HashMap클래스와 Hashtable 클래스 차이
![image](https://user-images.githubusercontent.com/89081374/211152280-18186a6c-5e57-4a25-8953-8c3d6d57dae8.png)

# HashMap 클래스
```java
java.lang.Object
    java.util.AbstractMap<K,V>
        java.util.HashMap<K,V>
```
### 인터페이스
```java
- Serializable : 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정
- Cloneable : Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정
              즉, 복제가 가능한 객체임을 의미
- Map<E> : 맵의 기본 메소드 지정
```
### 생성자
```java
- HashMap() : 16개의 저장 공간을 갖는 HashMap 객체를 생성
- HashMap(int initialCapacity) : 매개 변수만큼의 저장 공간을 갖는 HashMap 객체를 생성
- HashMap(int initialCapacity, float loadFactor) : 첫 매개 변수의 저장 공간을 갖고, 두 번째 매개 변수의 로드팩터를 갖는 HashMap객체를 생성
- HashMap(Map<? extend K,? extends V> m) : 매개 변수로 넘어온 Map을 구현한 객체에 있는 데이터를 갖는 HashMap 객체를 생성
  ```
- HashMap의 키는 기본 자료형과 참조 자료형 모두 될 수 있다.   
- 어떤 클래스를 만들어 그 클래스를 키로 사용할 때에는 Object 클래스의 hashCode() 메소드와 equals()메소드를 잘 구현해 놓아야 한다. 
- HashMap에 객체가 들어가면 hashCode() 메소드의 결과 값에 따른 bucket이라는 list 형태의 바구니가 만들어진다. 만약 서로 다른 키가 저장되어 있는데, hashCode() 메소드의 결과가 동일하다면, 이 버켓에 여러 개의 값이 들어갈 수 있다.   
  get()메소드가 호출되면 hashCode()의 결과를 확인하고, 버켓에 들어간 목록에 데이터가 여러 개일 경우 equals() 메소드를 호출해 동일한 값을 찾게 된다.
- 따라서, 키가 되는 객체를 직접 작성할 때에는 개발툴에서 제공하는 hashCode()와 equals()를 자동 생성해주는 기능을 사용해 해당 메소드를 꼭 구현해놓자.
### HashMap 객체의 값 확인 메소드
``` java
- Set<> keySet() : HashMap에 어떤 키가 있는지 확인
                   Set의 제네릭 타입은 키의 제네릭 타입과 동일하게 지정해 준다.
                   데이터를 저장한 순서대로 결과가 출력되지 않는다.
- Collection<> values : HashMap 객체에 담겨 있는 값만 필요할 경우
- entrySet() : Map에 선언된 Entry라는 타입의 객체 리턴, Entry에는 단 하나의 키와 값만이 저장된다.
```
``` java
  HashMap<String,String> map = new HashMap<String,String>();
  (생략)
  Set<String> keySet = map.keySet();
  Collection<String> values = map.values();
  Set<Map.Entry<String,String>> entries = map.entrySet();
  for(Map.Entry<String,String> tempEntry:entries){
    System.out.println(tempEntry.getKey()+"="+tempEntry.getValue());
  }
```
# TreeMap 클래스
- 저장하면서 키를 정렬한다.(숫자>알파벳대문자>알파벳소문자>한글)
- SortedMap 인터페이스를 구현했다.
# Properties 클래스
- Hashtable을 확장해서 Map 인터페이스에서 제공하는 모든 메소드를 사용할 수 있다.
- 기본적으로, 자바에서는 시스템의 속성을 이 클래스를 사용하여 제공한다.
## 시스템에서 제공하는 속성
- user.language : 사용자의 사용 언어
- user.dir : 현재 사용중인 기본 디렉터리
- user.home : 사용자 계정의 홈 디렉터리
- java.io.tmpdir : 자바에서 사용하는 임시 디렉터리
- file.encoding : 파일의 기본 인코딩
- sun.io.unicode.encoding : 유니코드 인코딩
- path.separator : 경로 구분자
- file.separator : 파일 구분자
- line.separator : 줄(line) 구분자
## 주요 메소드
``` java
  void load(InputStream inStream)
  void load(Reader reader) : 파일에서 속성을 읽는다
  void loadFromXML(InputStream in) : XML로 되어 있는 속성을 읽는다
  void store(OutputStream out, String comments)
  void store(Writer writer, String comments) : 파일에 속성을 저장한다.
  void storeToXML(OutputStream os, String comment)
  void storeToXML(OutputStream os, String comment, String encoding) : XML로 구성되는 속성 파일을 생성한다. 
```