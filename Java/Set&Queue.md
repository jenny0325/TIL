# Set
- 순서에 상관 없이, 어떤 데이터가 존재하는지 확인 위한 용도로 많이 사용
- 중복 방지하고, 원하는 값이 포함되어 있는지 확인하는 것이 주 용도
- Set 인터페이스를 구현한 주요 클래스
  - HashSet
    - 순서가 필요 없는 데이터를 해시 테이블에 저장
    - Set 중에 가장 성능이 좋다.
  - TreeSet
    - 저장된 데이터의 값에 따라 정렬되는 셋
    - red-black이라는 트리 타입으로 값이 저장
    - HashSet보다 약간 성능이 느리다
  - LinkedHashSet
    - 연결된 목록 타입으로 구현된 해시 테이블에 데이터를 저장
    - 저장된 순서에 따라 값이 정렬
    - 성능이 셋 중에 가장 나쁘다
- 성능 차이 발생 이유는 데이터 정렬 때문이다.
# HashSet
### 상속 관계
```java
java.lang.Object
  java.til.AbstractCollection<E>
    java.util.AbstractSet<E>
      java.util.HashSet<E>
```
### 구현한 인터페이스
``` java
- Serializable : 원격으로 객체를 전송하거나, 파일에 저장할 수 있음
- Cloneable : Object 클래스의 clone() 메소드가 제대로 수행될 수 있음
              즉, 복제가 가능한 객체임을 의미
- Iterable<E> : 객체가 foreach문을 사용할 수 있음
- Collection<E> : 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드
- Set<E> : 셋 데이터를 처리하는 것과 관련된 메소드
```
### 생성자
```java
- HashSet() : 데이터를 저장할 수 있는 16개의 공간과 0.75의 로드 팩터를 갖는 객체 생성
  *** 로드 팩터 : 데이터의 개수/저장공간
                데이터 개수가 증가해 로드 팩터보다 커지면, 해시 재정리 작업을 해야한다.
                이때, 내부에 갖고 있던 자료 구조를 다시 생성하는 단계를 거쳐 성능에 영향이 발생한다.
- HashSet(Collection<? extends E> c) : 매개 변수로 받은 컬랙션 객체의 데이터를 HashSet에 담는다.
- HashSet(int initalCapacity) : 매개 변수로 받은 개수만큼의 데이터 저장 공간과 0.75의 로드 팩터를 갖는 객체를 생성
- HashSet(int initalCapacity, float loadFactor) : 첫 매개 변수로 받은 개수만큼의 데이터 저장 공간과 두 번째 매개 변수로 받은 만큼의 로드 팩터를 갖는 객체 생성
```
### 주요 메소드
```java
- boolean add(E e) : 데이터 추가
- void clear() : 모든 데이터 삭제
- Object clone() : HashSet 객체 복제. 하지만, 담겨 있는 데이터들은 복제하지 않는다.
- boolean contains(Object o) : 지정한 객체 존재하는지 확인
- boolean isEmpty() : 데이터가 있는지 확인
- Iterator<E> iterator() : 데이터를 꺼내기 위한 Iterator 객체를 리턴
- boolean remove(Object o) : 매개 변수로 넘어온 객체를 삭제
- int size() : 데이터 개수 리턴
```
# Queue
- FIFO (First In First Out)
- Deque : Double Ended Queue
  - Queue 인터페이스를 확장
  - 맨 앞에 값을 넣거나 뺴는 작업, 맨 뒤에 값을 넣거나 뺴는 작업을 수행하는데 용이
- LinkedList
  - List이면서 Queue, Deque이다.
  - 배열의 중간에 있는 데이터가 지속적으로 삭제되고, 추가될 경우
  - LinkedList가 배열보다 메모리 공간 측면에서 유리하다.
  - 중간에 있는 데이터를 삭제하면, 지운 데이터의 앞에 있는 데이터와 뒤에 있는 데이터를 연결하기만 하면 된다. 위치를 위해 값을 이동할 필요가 없다
### 상속 관계
``` java
java.lang.Object
  java.util.AbstractCollection<E>
    java.util.AbstractList<E>
      java.util.AbstractSequentialList<E>
        java.util.LinkedList<E>
```
### 구현한 인터페이스
```java
  - Serializable : 원격으로 객체를 전송하거나, 파일에 저장할 수 있음
  - Clonable : Object 클래스의 clone() 메소드가 제대로 수행될 수 있음
    즉, 복제가 가능한 객체임을 의미
  - Iterable<E> : 객체가 foreach문을 사용할 수 있음
  - Collection<E> : 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정
  - Deque<E> : 맨 앞과 맨 뒤의 값을 용이하게 처리하는 큐와 관련된 메소드 지정
  - List<E> : 목록형 데이터를 처리하는 것과 관련된 메소드 지정
  - Queue<E> : 큐를 처리하는 것과 관련된 메소드 지정
```
### 생성자
```
  - LinkedList() : 비어 있는 LinkedList 객체 생성
  - LinkedList(Collectio<? extends E> c) : 매개 변수로 받은 컬렉션 객체의 데이터를 LinkedList에 담는다. 
```
### LinkedList 데이터 추가
```java
    void addFirst(Object)    ★ 권장
    boolean offerFirst(Object)
    void push(Object) : LinkedList 객체의 가장 앞에 데이터를 추가
    boolean add(Object)
    void addLast(Object)   ★ 권장
    boolean offer(Object)
    boolean offerLast(Object) : LinkedList 객체의 가장 뒤에 데이터 추가  
    void add(int, Object) : LinkedList 객체의 특정 위치에 데이터 추가
    Obejct set(int, Object) : LinkedList 객체의 특정 위치에 있는 데이터 수정 후 기존에 있던 데이터 리턴
    boolean addAll(Collection) : 매개 변수로 넘긴 컬렉션의 데이터 추가
    boolean addAll(int, Collection) : 매개 변수로 넘긴 컬렉션의 데이터를 지정된 위치에 추가
```
### LinkedList 데이터 꺼내기
``` java
    Object getFirst()   ★ 권장
    Object peekFirst()
    Object peek()
    Object element() : LinkedList 객체의 맨 앞에 있는 데이터 리턴
    Object getLast()
    Object prrkLast() : LinkedList 객체의 맨 뒤에 있는 데이터 리턴
    Object get(int) : LinkedList 객체의 지정한 위치에 있는 데이터 리턴, LinkedList 데이터 포함 확인
    boolean contains(Object) : 매개 변수로 넘긴 데이터가 있을 경우 true 리턴
    int indexOf(Object) : 매개 변수로 넘긴 데이터의 위치를 앞에서부터 검색해 리턴, 없으면 -1 리턴
    int lastIndexOf(Object) : 매개 변수로 넘긴 데이터의 위치를 끝에서부터 검색해 리턴, 없을 경우 -1 리턴
```
###  LinkedList 데이터 삭제
```java
    Object remove()
    Object removeFirst()  ★ 권장
    Object poll()
    Object pollFirst()
    Object pop() : LinckedList 객체의 가장 앞에 있는 데이터를 삭제하고 삭제된 데이터 리턴
    Object pollLast()
    Object removeLast()  ★ 권장  : LinkedList 객체의 가장 끝에 있는 데이터를 삭제하고 삭제된 데이터 리턴
    Object remove(int) : 매개 변수에 지정된 위치에 있는 데이터 삭제 후 삭제된 데이터 리턴
    boolean remove(Object)
    boolean removeFirstOccurrence(Object) : 매개 변수로 넘겨진 객체와 동일한 데이터 중 앞에서부터 가장 처음에 발견된 데이터 삭제
    boolan removeLastOccurrence(Object) : 매개 변수로 넘겨진 객체와 동일한 데이터 중 끝에서부터 가장 처음에 발견된 데이터 삭제
```
### LinkedList 데이터 검색
``` java
    ListIterator listIterator(int) ; 매개 변수에 지정된 위치부터의 데이터를 검색하기 위한 ListIterator 객체를 리턴
    Iterator descendingIterator() : ListIterator의 데이터를 끝에서부터 검색하기 위한 Iterator 객체를 리턴
    ListIterator : Iterator 인터페이스가 다음 데이터만을 검색할 수 있다는 단점을 보완해 이전 데이터도 검색할 수 있는 Iterator다.
```