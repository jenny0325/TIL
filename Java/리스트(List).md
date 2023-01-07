# 자바 컬렉션
- 목록성 데이터를 처리하는 자료 구조  
- 자료 구조 (Data Structure) : 여러 데이터를 담을 때 사용  
  - List : 순서가 있는 리스트
  - Set : 순서가 중요하지 않은 셋
  - Queue : 먼저 들어온 것이 먼저 나가는 큐
  - Map : key-value로 저장되는 맵
  - List, Set, Queue는 java.util.Collection 인터페이스를 구현한다.
  ![image](https://user-images.githubusercontent.com/89081374/211149146-1c6a0da4-55d7-4c63-98e5-beae4b13f7a0.png)

# Collection 인터페이스
- 구조
```java
public interface Collection<E> extends Iterable<E>
```
```java
- 주요 메소드
   - boolean add(E e)
      요소를 추가
    - boolean addAll(Collection)
      컬렉션의 모든 요소를 추가
    - void clear()
      컬렉션에 모든 요소 데이터를 지운다.
    - boolean contains(Object)
      매개 변수로 넘어온 객체가 해당 컬렉션에 있는지 확인. 있으면 true 리턴
    - boolean containsAll(Collection)
      매개변수로 넘어온 객체들이 해당 컬렉션에 있는지 확인. 모두 있으면 true 리턴
    - boolean equals(Object)
      매개 변수로 넘어온 객체와 같은 객체인지 확인
    - int hashCode()
      해시 코드값을 리턴
    - boolean isEmpty()
      컬렌션이 비어있는지 확인. 비어있으면 true 리턴
    - Iterator iterator()
      데이터를 한 건씩 처리하기 위한 Iterator 객체를 리턴
    - boolean remove(Object)
      매개 변수와 동일한 객체 삭제
    - boolean removerAll(Collection)
      매개변수로 넘어온 객체들을 해당 컬렉션에서 삭제
    - boolean retainAll(Collection)
      매개 변수로 넘어온 객체들만 컬렉션에 남겨 둔다
    - int size()
      요소의 개수를 리턴
    - Object[] toArray()
      컬렉션에 있는 데이터들을 배열로 복사
    - <T> t[] toArray(T[])
      컬렉션에 있는 데이터들을 지정한 타입의 배열로 복사 
```
 
- Iterable 인터페이스
```java
Iterator<T> iterator()
```
- Iterator 인터페이스
```java
- 구조
  public interface Iterator<E>

- 메소드
    - boolean hasNext()
    - E next()
    - void remove()
```
# List
- List 인터페이스는 Collection인터페이스를 확장했다.
- 주로 ArrayList, Stack, LinkedList를 많이 사용한다. 
- ArrayList는 Thread safe 하지 않고, Vector는 Thread safe하다.
- Stack 
  - Vector 클래스를 확장했다.
  - LIFO (Last In First Out)
- LinkedList
  - 목록, 큐에 속한다
# ArrayList
### 상속 관계
```java
java.lang.Object 
     java.util.AbsractCollection<E>
         java.util.AbstractList<E>
             java.util.ArrayList<E>
```
### 구현한 인터페이스들
``` java
  - Serializable : 원격으로 객체를 전송하거나,파일에 저장할 수 있음을 지정
  - Cloneable : Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정
    즉, 복제가 가능한 객체임을 의미
  - Iterable<E> : 객체가 foreach문을 사용할 수 있음을 지정
  - Collection<E> : 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정
  - List<E> : 목록형 데이터를 처리하는 것과 관련된 메소드 지정
  - RandomAccess : 목록형 데이터에 보다 빠르게 접근할 수 있도록
    임의로 접근하는 알고리즘이 적용된다는 것을 지정
```
### 생성자
``` java
- ArrayList() : 객체를 저장할 공간이 10개인 ArrayList를 만든다. 
                매개 변수를 넣지 않으면 초기 크기는 10이다. 10개 이상의 데이터가 들어오면
                크기를 늘이는 작업이 ArrayList 내부에서 자동으로 수행된다.
- ArrayList(Collection<? extends E> c) : 매개변수로 넘어온 컬렉션 객체가 저장되어 있는 ArrayList를 만든다.
- ArrayList(int initialCapacity) : 매개 변수로 넘어온 initialCapacity 개수만큼의 저장 공간을 갖는 ArrayList를 만든다.
```
 - 보통 한 가지 타입의 객체를 저장한다.
 - 컬렉션 관련 클래스의 객체들을 선언할 때에는 제네릭을 사용해 선언하는 것을 권장
```java
ex) ArrayList<String> lis1 = new ArrayList<String>();
```
### ArrayList 데이터 추가
``` java
boolean add(E e) : 매개 변수로 넘어온 데이터를 가장 끝에 담는다.
void add(int index, E e) : 매개 변수로 넘어온 데이터를 지정된 index위치에 담는다.
boolean addAll(Collection<? extends E> c) : 매개 변수로 넘어온 컬렉션 데이터를 가장 끝에 담는다.
boolean addAll(int index, Collection<? extends E> c) : 매개 변수로 넘어온 컬렉션 데이터를 index 위치부터 담는다.
```
### ArrayList 데이터 꺼내기
``` java
int size() : ArrayList 객체에 들어가 있는 데이터의 개수를 리턴
E get(int index) : 매개 변수에 지정한 위치에 있는 데이터 리턴 
int indexOf(Object o) : 매개 변수로 넘어온 객체와 동일한 데이터의 위치를 리턴
int lastIndexOf(Object o) : 매개 변수로 넘어온 객체와 동일한 마지막 데이터의 위치를 리턴
Object[] toArray() : ArrayList객체에 있는 값들을 Object[] 타입의 배열로 만듦
<T> T[] toArray(T[] a) : ArrayList객체에 있는 값들을 매개 변수로 넘어온 T타입의 배열로 만듦
                         ArrayList객체의 크기가 매개변수 배열 객체의 크기보다 클 경우 매개 변수 배열의 모든 값이 null로 채워진다. 
                         그러므로, 크기가 0인 배열을 넘겨주는 것이 가장 좋다. 
                         ex) String[] strList = list.toArray(new String[0]);
```
### ArrayList 데이터 삭제
```java
void clear() : 모든 데이터를 삭제 
E remove(int index) : 매개 변수에서 지정한 위치에 있는 데이터를 삭제하고 삭제한 데이터를 리턴 
boolean remove(Object o) : 매개 변수에서 넘어온 객체와 동일한 첫 번째 데이터 삭제 
boolean removeAll(Collection<?> c) : 매개 변수로 넘어온 컬렉션 캑체에 있는 데이터와 동일한 모든 데이터 삭제
```
### ArrayList 데이터 변경
``` java
  E set(int index, E element) : index 위치에 데이터를 두 번째 매개 변수로 넘긴 값으로 변경, 해당 위치에 있던 데이터를 리턴
```
# Stack
- LIFO 기능을 구현할 때 사용하는 클래스
### 상속 관계
```
  java.lang.Object
       java.util.AbstractCollection<E>
            java.util.AbstractList<E>
                java.util.Vector<E>
                    java.util.Stack<E>
```
### 구현한 인터페이스
```
    - Serializable
    - Cloneable
    - Iterable<E>
    - Collection<E>
    - List<E>
    - RandomAccess
```
### 생성자
```
      Stack() : 아무 데이터도 없는 Stack 객체를 만듦
      메소드
      boolean empty() : 객체가 비어있는지 확인
      E peek() : 객체의 가장 위에 있는 데이터 리턴
      E pop() : 객체의 가장 위에 있는 데이터 지우고 리턴
      E push(E item) : 매개 변수로 넘어온 데이터를 가장 위에 저장
      int search(Object o) : 매개 변수로 넘어온 데이터의 위치 리턴
```