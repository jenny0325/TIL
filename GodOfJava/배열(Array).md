# 배열(Array)
배열은 한 가지 타입에 대해서, 하나의 변수에 여러개의 데이터를 넣을 수 있다.

배열을 선언할 때 대괄호는 타입과 변수 사이에 위치해도 되고, 변수명 뒤에 위치해도 되지만 타입과 변수명 사이에 위치하는 것을 권장한다.

```java
int [] odds;
ods = new int[5];

int [] odds = new int[5];

int [] odds = {1, 3, 5, 7, 9};

//중괄호를 통해서 선언할때는 무조건 한줄로 선언해야한다. 아래는 컴파일 오류 발생
int [] odds;
ods = {1, 3, 5, 7, 9}

// 길이에 대한 숫자값이 없으므로 컴파일 오류가 발생한다.
int [] ods = new int[];   
```

## 배열의 길이
배열은 보통 for문과 함께 사용된다. for문에 배열이 사용될 경우 배열의 길이만큼 for 문을 반복하는 것이 중요한데 배열의 길이는 다음과 같이 length를 이용하여 구한다.
```java
String[] weeks = {"월", "화", "수", "목", "금", "토", "일"};
for (int i = 0; i < weeks.length; i++) {
    System.out.println(weeks[i]);
}

System.out.println(weeks[7]);  // 8번째 배열값이 없으므로 ArrayIndexOutOfBoundsException 오류가 발생한다.
```

## 배열을 위한 for 루프

```java
for (타입이름 임시변수명 : 반복대상객체){
    
}

int [] odds = {1, 3, 5, 7, 9};
for(int data: odds) {
    System.out.println(data);    
}
```

## 2차원 배열
2차원 이상의 배열을 자주 사용되지 않는다.

```java
int [][] twoDim;
twoDim = new int[2][3];

twoDim = new int[2][]; //컴파일 에러가 나지 않지만 추후에 2차원 배열의 크기를 선언해주어야 한다!

twoDim = new int[][3]; // 컴파일 에러
twoDim = new int[][]; // 컴파일 에러

int [][] twoDim = {{1, 2, 3}, {4, 5, 6}};
```