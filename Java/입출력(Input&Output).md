# I/O
- JVM 기준으로 Input, Output
- 파일을 읽거나 저장할 때, 다른 서버나 디바이스로 보낼 때 사용
# File클래스
- 파일 및 경로 정보를 통제하기 위한 클래스
- 생성한 파일 객체가 가리키고 있는 것이 존재하는지, 파일인지 경로인지, 읽거나 쓰거나 실행할 수 있는지 언제 수정되었는지 확인하는 기능을 제공
- 해당 파일의 파일의 이름을 바꾸고, 삭제, 생성, 전체 경로를 확인하는 기능을 제공한다.
- 경로를 가리킬 경우엔 파일의 목록을 가져오거나, 경로를 생성하고, 삭제하는 등의 기능이 있다.
- JDK 7 이상의 버전을 사용하면, java.io.file 패키지에 Files 클래스를 사용하는 것이 더 효과적이다. 파일을 보다 효율적으로 처리하기 위해 만들어졌으며, 기존에 여러 단점들을 보완한다.
  - File클래스 : 객체를 생성하여 데이터를 처리
  - Files클래스 : 모든 메소드가 static 으로 선언되어 있어서 별도의 객체를 생성할 필요 없다
## 생성자
```java
- File(File parent, String child) : 이미 생성되어 있는 File 객체(parent)와 그 경로의 하위 경로 이름으로 새로운 File객체 생성
- File(String pathname) : 지정한 경로 이름으로 file 객체 생성
- File(String parent, String child) : 상위 경로(parent)와 하위 경로(child)로 File 객체 생성
- File(URI uri) : URI에 따른 File 객체를 생성
        
*** child라고 되어 있는 값은 경로가 될 수도 있고, 파일 이름이 될 수 있다.
```
### 파일의 경로와 상태를 확인하는 메소드
```java
- boolean exists() : 해당 경로가 존재하는지
- boolean mkdir() : 디렉터리를 하나만 만든다
- boolean mkdirs() : 여러 개의 하위 디렉터리를 만든다
- boolean isDirectory() : 해당 객체가 경로를 나타내는지
- boolean isFile() : 해당 객체가 파일을 나타내는지
- boolean isHidden() : 해당 객체가 숨긴 파일인지
- boolean canRead() : 현재 수행중인 자바 프로그램이 해당 File객체에 읽을 수 있는 권한이 있는지
- boolean canWrite() : 현재 수행중인 자바 프로그램이 해당 File객체에 쓸 수 있는 권한이 있는지
- boolean canExecute() : 현재 수행중인 자바 프로그램이 해당 File객체를 실행할 수 있는 권한이 있는지, JAVA6부터 추가
- long lastModified() : 파일이나 경로가 언제 생성되었는지 long타입의 현재 시간 리턴
- boolean delete() : 파일을 삭제
```

### 파일을 처리하는 메소드
```java
- boolean createNewFile()  : 파일 생성했는지 확인. 이미 존재하면 fale 리턴 , IOException을 던진다
- File getAbsoluteFile(), File getCanonicalFile() : File 객체 리턴
- String getAbsolutePath(), String getCanonicalPath() : 전체 경로 String 리턴
- String getName() : 파일일 경우 파일의 이름, 경로는 전체 경로 리턴
- String getPath() : 경로+파일 이름 리턴
- String getPatent() : 객체가 File을 가리키고 있다면, 파일 이름을 제외한 경로만 리턴
```
### 디렉터리 목록 확인을 위한 list 메소드들
```java
- static File[] listRoots() : JVM이 수행되는 OS에서 사용중인 파일 시스템의 루트 디렉터리 목록을 File 배열로 리턴
- String[] list() : 현재 디렉터리의 하위 목록을 리턴
- String[] list(FilenameFilter filter) : 현재 디렉터리의 하위 목록 중, filter 조건에 맞는 목록 String 배열로 리턴
- File[] listFiles() : 현재 디렉터리의 하위에 있는 목록을 File 배열로 리턴
- File[] listFiles(FileFilter filter) :현재 디렉터리 하위 목록 중, filter조건에 맞는 목록 File 배열로 리턴
- File[] listFiles(filenameFilter filter) : 현재 리렉터리 하위 목록 중, filter 조건에 맞는 목록 File배열로 리턴

*** FileFilter,filenameFilter 인터페이스의 aceept메소드를 구현해야한다.

예시)
public clss JPGFileFilter implements FileFilter {
  @Override
  public boolean accept(File file) {
      if(file.isFile()) {
          String fileName = file.getName();
          if(fileName.endWith(".jpg")) return true;
      }
      return false;
  }
}

예시)
public class JPGFilenameFilter implements FilenameFilter { 
    @Override 
    public boolean accep(File file, String fileName) {
      if(fileName.endWith(".jpg")) return true;
      return false;
    }
}
```


