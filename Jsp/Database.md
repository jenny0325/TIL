# 데이터베이스란?
- 데이터베이스: 여러 사람에 의해 공유되어 사용될 목적으로 통합하여 관리되는 데이터의 집합
  - 주요 목적: 데이터를 저장하고, 필요할 때 사용하는 것
- DBMS(Database Management System): 데이터베이스를 관리하는 시스템
- DBMS가 제공하는 여러 기능들:
  - 데이터의 추가/조회/변경/삭제
  - 데이터의 무결성(integrity) 유지
  - 트랜잭션 관리: 데이터의 신뢰성을 높임
  - 데이터의 백업 및 복원
  - 데이터 보안
-  웹 어플리케이션을 구축할 때 주로 사용하는 데이터베이스는 관계형 데이터베이스(RDBMS)

# 테이블과 레코드
- 테이블: RDBMS에서 데이터를 저장하는 장소
- 스키마: 테이블 구조와 관련된 정보 (예:테이블의 어떤 데이터를 저장하며, 그 데이터의 길이는 최대 몇 글자인지 등)
- 테이블 구조는 각각의 정보를 저장하는 칼럼과 칼럼 타입, 각 칼럼의 길이로 구성
- 예시 - 회원 정보를 저장하는 테이블의 스키마  
![img](https://user-images.githubusercontent.com/89081374/212539584-3fa30409-1a6b-4859-a58c-7af70e8e3efa.png)
- 스키마는 하나의 데이터에 대한 구조를 나타냄  
<img width="472" src="https://user-images.githubusercontent.com/89081374/212539677-602e2611-ad1e-4e61-8971-217b5bd74d12.png">
- 레코드: 칼럼 데이터 모음 (위의 MEMBERID, PASSWORD, NAME, EMAIL 모음)
- 하나의 테이블은 여러 개의 레코드로 구성됨 
- 데이터베이스 프로그래밍: 레코드, 칼럼, 테이블을 사용해서 데이터를 저장하고 조회하는 작업을 수행하는 것

# 주요키(Primary Key)와 인덱스(Index)
- 레코드를 미리 특정 값을 이용해 정렬해 놓으면 더 빠르게 레코드를 찾을 수 있고, 이러한 목적으로 사용할 수 있는 것이 주요키(Primary Key) 칼럼임
- 주요키 컬럼: 하나의 테이블에 저장된 모든 레코드가 서로 다른 값을 갖는 칼럼
- 인덱스: 레코드의 특정 칼럼을 사용해서 레코드를 쉽게 찾을 수 있도록 미리 정리된 표로 만들어 두는 것
  - 주요키와 더불어 레코드를 분류할 때 사용
  - 주요키도 인덱스의 일종이지만, 인덱스는 중복된 값에 대한 정렬이 가능하다는 차이가 있음
  - 사용 예: 회원 이름으로 데이터를 조회하는 기능이 많은 경우, 회원 이름 칼럼을 사용한 인덱스를 생성하면 회원 이름으로 빠르게 데이터 조회 가능

# 데이터베이스 프로그래밍의 일반적인 순서
![img](https://user-images.githubusercontent.com/89081374/212539739-cf257763-e2bd-40ab-881d-81a785fad15a.png)

# 데이터베이스 프로그래밍의 필수 요소
- 데이터베이스 프로그래밍에는 세 가지 요소가 필요함
  - DBMS: 데이터베이스를 관리해주는 시스템 ex) 오라클, MySQL
  - 데이터베이스: 데이터를 저장할 공간
    - 데이터베이스를 생성하는 방법은 DBMS마다 다르고, 사용할 DBMS에 맞게 생성해야함
  - DBMS 클라이언트: 데이터베이스를 사용하는 애플리케이션
    - 예: MySQL의 mysql.exe, MySQL Workbench / 자바의 JDBC Driver
    
# 데이터베이스 생성
- 아래 예제들을 실행하기 위한 MySQL DBMS(AWS RDS) 설치 및 워크벤치 연결 - https://scshim.tistory.com/218
- 데이터베이스 생성:
```sql
create database chap14 default character set utf8
```
- MySQL에서 사용할 사용자 추가:
```sql
CREATE USER 'jspexam'@'localhost' IDENTIFIED BY 'jsppw';
GRANT ALL PRIVILEGES ON chap14.* TO 'jspexam'@'localhost';

CREATE USER 'jspexam'@'%' IDENTIFIED BY 'jsppw';
GRANT ALL PRIVILEGES ON chap14.* TO 'jspexam'@'%';
```
- CREATE USER 쿼리는 새로운 계정을 생성함
  - 호스트 값이 localhost이면 localhost에서 해당 계정에 접근할 때 해당 암호를 사용한다는 의미고, 호스트 값이 '%'인 경우 모든 호스트에서 해당 계정으로 접근할 때 해당 암호를 사용한다는 것을 뜻함
```sql
create user '[계정]'@'[호스트]' identified by '[암호]'
```
- GRANT 쿼리는 MySQL DBMS 계정에 권한을 부여함
  - 권한이 all privileges이면 모든 권한을 부여함
```sql
grant [권한목록] on [데이터베이스].[대상] to '[계정]'@'[호스트]'
```
- 특정 권한만 부여하고 싶다면 개별 권한 목록을 지정함
```sql
grant select, insert, update, delete, create, drop on chap14.* to 'jspexam'@'%';
```
- 권한 부여 대상을 전체로 하고 싶다면 '*'  값을 사용
```sql
grant all privileges on chap14.* to "jspexam'@'%';
```