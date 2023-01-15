# SQL 이란?
- SQL(Structured Query Language): 데이터를 조회하고 삭제하는 등의 작업을 수행할 때 사용하는 언어
- 주요 SQL 타입

|SQL 타입| 설명|
|-------|----|
|CHAR| 확정 길이의 문자열을 저장한다, 표준의 경우 255글자까지만 저장 가능|
|VARCHAR| 가변 길이의 문자열을 저장, 표준의 경우 255글자까지만 저장 가능|
|LONG VARCHAR|  긴 가변 길이의 문자열을 저장|
|NUMERIC|  숫자를 저장|
|DECIMAL| 십진수를 저장|
|INTEGER|  정수를 저장|
|TIMESTAMP| 날짜와 시간을 저장|
|TIME|  시간을 저장|
|DATE| 날짜를 저장|
|CLOB| 대량의 문자열 데이터를 저장|
|BLOB|  대량의 이진 데이터를 저장|
- DBMS는 표준 SQL에 더불어 자체적으로 확장 타입을 제공함
  - 오라클은 4,000바이트까지 저장 가능한 VARCHAR2라는 타입을 제공함 (오라클에서 VARCHAR와 VARCHAR2는 동일)
  - MySQL은 Timestamp 타입뿐만 아니라 DATETIME 타입을 제공
  - 오라클의 VARCHAR처럼 표준 SQL과 이름은 같지만 완전히 다른 경우도 존재

# 테이블 생성 쿼리
- 예시 - 테이블을 생성할 때 사용하는 쿼리 형식
```sql
create table TABLENAME (
    COL_NAME1	COL_TYPE1(LEN1),
    COL_NAME2	COL_TYPE2(LEN2),
    ...
    COL_NAMEn	COL_TYPEn(LENn)
)
```
- TABLENAME: 테이블을 식별할 때 사용할 이름 
- COL_NAME: 각 칼럼의 이름 
- COL_TYPE: 각 칼럼에 저장될 값의 타입 
- LEN: 저장될 값의 최대 길이


- 예시 - MEMBER라는 이름의 테이블 생성
```sql
create table MEMBER (
    MEMBERID VARCHAR(10),
    PASSWORD VARCHAR(10),
    NAME VARCHAR(20),
    EMAIL VARCHAR(80)
)
```
- 위 테이블의 문제점: 주요키에 대한 정보 표시 X, 필수 값에 대한 여부 표시 X 
- 주요키 칼럼을 표시하는 방법: 칼럼 타입 뒤에 'PRIMARY KEY' 문장 추가 
- 필수 값을 표시하는 방법: 칼럼 타입 뒤에 'NOT NULL' 문장 추가
```sql
create table MEMBER (
    MEMBERID VARCHAR(10) NOT NULL PRIMARY KEY,
    PASSWORD VARCHAR(10) NOT NULL
    NAME VARCHAR(20) NOT NULL,
    EMAIL VARCHAR(80)
)
```
- MySQL에서는 테이블 저장 엔진과 캐릭터 셋을 따로 설정하면서 테이블을 생성할 수 있음
```sql
create table MEMBER (
    MEMBERID VARCHAR(10) NOT NULL PRIMARY KEY,
    PASSWORD VARCHAR(10) NOT NULL
    NAME VARCHAR(20) NOT NULL,
    EMAIL VARCHAR(80)
) engine=InnoDB default character set = utf8;
```
- 위 쿼리를 통해 생성된 테이블은 InnoDB 엔진과 UTF-8 캐릭터 셋을 사용함

# 테이블 삽입 쿼리
- 테이블에 데이터를 추가할 때는 INSERT 쿼리 사용
```sql
insert into [테이블이름] ([칼럼1], [칼럼2], ... [칼럼n])
values ([값1], [값2], ... [값n])
```
- [테이블이름]: 레코드를 삽입할 테이블 이름
- [칼럼]: 값을 입력할 칼럼의 이름
- [값]: 해당 칼럼의 값

- 예시 - MEMBER 테이블에 레코드 삽입
```sql
insert into MEMBER (MEMBERID, PASSWORD, NAME)
values ('madvirus', '1234', '최범균');
```
- 칼럼의 값을 지정하지 않으면 null 값이 들어감 (위 쿼리에서 MEMBER 테이블의 EMAIL 칼럼이 생략 -> EMAIL 칼럼 값에 null 삽입)
- SQL에서는 작은따옴표를 사용하여 문자열을 표시
  - 값 자체에 작은따옴표가 있으면 문제가 발생함
- 예시 - 작은따옴표가 포함된 문자열로 인한 에러
```sql
insert into MEMBER values('era13', '5678', '최'범균', 'madvirus2@gmail.com');
```
- 칼럼 목록을 표시하지 않으면 전체 칼럼에 대해 값을 지정해야함
```sql
insert into MEMBER values('eral3', '5678', '최범균', 'madvirus.net');
```

# 데이터 조회 쿼리 - 조회 및 조건
- 테이블에 저장된 데이터를 조회할 때는 SELECT 쿼리를 사용
```sql
select [칼럼1], [칼럼2], ..., [칼럼n] from [테이블이름]
```
- [칼럼]: 읽어오고자 하는 칼럼의 목록
- 전체 칼럼을 모두 읽고 싶으면 전체 칼럼 이름을 적는 대신, 별표('*')를 사용
```sql
select * from MEMBER;
```
- SELECT 쿼리는 기본적으로 모든 레코드의 목록을 읽어 옴
- 특정 조건을 충족하는 레코드만 필요한 경우 WHERE 절을 사용
  - WHERE 절은 FROM 부분 뒤에 옴
```sql
select * from MEMBER where NAME = '최범균';
```
- 같지 않음을 표현할 때는 '<>' 연산자를 사용 ('<>'은 sql 표준이고, 비표준으로 '!='도 가능)
- 예시 - EMAIL 칼럼의 값이 빈 문자열('')이 아닌 레코드를 검색
```sql
select * from MEMBER EMAIL <> '';
```
- NULL이거나 NULL이 아닌 레코드를 구하고 싶다면, NULL 관련 전용 쿼리를 사용
```sql
select * from MEMBER where EMAIL is NULL;
select * from MEMBER where EMAIL is not NULL;
```
- 값을 비교하는 부등호기호 사용 가능 (<, >, <=, >=)
```sql
where SALARY >= 1000 and SALARY <= 2000
```
-  LIKE 조건은 특정 문자를 포함하는지 검사함
```sql
where NAME like '최%';
```
- '%'는 모든 것을 의미하고, 위 조건은 NAME 칼럼의 값이 '최*****'의 형태를 갖는지 여부를 판단함
- '%범%'과 같이 조건을 주면 앞뒤로 모든 문장이 오고 중간에 '범'이 포함된 값인지 여부를 판단
- 주의! LIKE는 검색 속도가 매우 느리므로, (수백만 이상의 레코드가 존재한다면) 빠른 검색 속도를 위해 별도의 검색 엔진을사용하자.

# 데이터 쿼리 조회 - 정렬
- ORDER BT절을 사용해서 데이터 정렬을 처리함
  - 정렬을 원하는 칼럼 이름 뒤에 'asc'(오름차순)나 'desc'(내림차순)를 붙임
- 여러 개의 칼럼을 사용하면 지정한 칼럼 순서대로 정렬함
```sql
select .. from [테이블이름] where [조건절] order by [칼럼1] asc, [칼럼2] desc, ...;
```
- 예시 - Name 칼럼으로 오름차순 정렬 후 MEMBERID 칼럼으로 내림차순 정렬
```sql
select * from MEMBER order by NAME asc, MEMBERID desc;
```
- NAME 칼럼으로 정렬된 상태가 되면, NAME 칼럼이 같은 값을 갖는 것들에 대해서만 MEMBERID 칼럼으로 정렬함

# 데이터 쿼리 조회 - 집합
- 집합 관련 함수로 총합, 최대, 최소, 개수를 구할 수 있음
  - 총합: sum(), 최대: max(), 최소: min(), 개수: count()
- 예시 - SALARY 칼럼의 가장 큰 값, 가장 작은 값, 전체 레코드 칼럼의 합 구하기
```sql
select max(SALARY), min(SALARY), sum(SALARY) from ...;
```
- 예시 - 전체 레코드 개수 구하기
```sql
select count(*) from MEMBER;
```

# 데이터 수정 쿼리
- 데이터를 수정할 때는 UPDATE 쿼리를 사용함
```sql
update [테이블이름] set [칼럼1]=[값1], [칼럼2]=[값2], ... where [조건절];
```
- 주의! UPDATE 쿼리를 실행할 때 WHERE 절을 알맞게 사용하지 않으면, 전체 레코드가 변경됨. 따라서, 반드시 where 사용할 것
- 예시 - 다음과 같이하면, 전체 레코드가 변경됨
```sql
update MEMBER set NAME='최범균';
```

# 데이터 삭제 쿼리
- 데이터를 수정할 때는 DELETE 쿼리를 사용함
```sql
delete from [테이블이름] where [조건절]
```
- 주의! DELETE 쿼리를 실행할 때 WHERE 절을 알맞게 사용하지 않으면, 전체 레코드가 변경됨. 따라서, 반드시 where 사용할 것
- 예시 - 다음과 같이하면, 전체 레코드가 삭제됨
```sql
delete from MEMBER
```

# 조인(Join)
- 조인: 두 개 이상의 테이블로부터 같은 값을 갖는 칼럼을 사용하여 함께 레코드를 묶어서 읽어오는 것
- 두 개 이상의 테이블에서 관련 있는 데이터를 함께 읽어올 때 사용
```sql
select A. 칼럼1, A. 칼럼2, B. 칼럼3, B. 칼럼4
from [테이블1] as A, [테이블2] as B
where A.[칼럼x] = B.[칼럼y]
```
- 위 쿼리에서 [테이블1] as A는 테이블1을 A로 표시한다는 것을 의미, B는 [테이블2]를 나타냄
- 이 글에서는 내부 조인에 대해서만 설명하고, 더 다양한 형태의 조인이 존재함