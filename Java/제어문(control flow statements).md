# 제어문이란?(control flow statements)
자바 프로그램이 원하는 결과를 얻기 위해서는 프로그램의 순차적인 흐름을 제어해야만 할 경우가 생긴다.  
이때 사용하는 명령문을 제어문이라고 하며, 이러한 제어문에는 조건문, 반복문 등이 있다.

# 조건문(conditional statements)
조건문은 주어진 조건식의 결과에 따라 별도의 명령을 수행하도록 제어하는 명령문이다.  
자바에 사용하는 조건문은 다음과 같은 것이 있다.
1. if 문
2. if - else 문
3. if - else if - else 문
4. switch 문

## if문
```java
if (조건식) {
수행문;
}

// 블럭 안에 수행문이 하나일 경우 괄호를 생략 가능하다.
if (조건식) 수행문;
```

조건식이 '참'일 경우 블럭 안의 문장이 수행된다.

조건식의 결과가 반드시 true 또는 false이어야 하므로 비교연산자와 논리연산자로 구성된다.
```java
if (조건식) {
수행문 1;   // 조건식이 참일 경우 수행.
} else {
수행문 2;   // 조건식이 거짓일 경우 수행.
}
// 조건연산자
(조건식) ? 결과1 : 결과2;
// 조건식이 참일경우 결과1, 거짓일 경우 결과2.

```


```java
if - else
if (조건식 1) {
수행문 1;   // 조건식 1이 참일 경우 수행.
} else if (조건식 2) {
수행문 2;   // 조건식 2가 참일 경우 수행.
} else if (조건식 3) {
수행문 3;   // 조건식 3이 참일 경우 수행.
} else {
수행문 4;   // 조건식 1~3에 모두 해당하지 않을 경우 수행.
};
```

조건이 여러 개일 경우 if - else문으로 표현 가능하다.

첫 번째 조건부터 순서대로 평가해서 결과가 참인 조건식을 만나면 해당 블럭을 수행하고 if - else문을 벗어난다.

결과가 참인 조건식이 하나도 없으면 else블럭의 문장이 수행된다. else 블럭은 생략이 가능하다.




## switch문
~~~java
switch (조건) {
case 값1:
수행문1;
break;

case 값2:
수행문2;
break;

case 값3:
수행문3;
break;

default:
수행문4;
}
~~~

경우의 수가 많아지면 if-else문 대신 switch문으로 작성하는 것이 좋다.

switch문의 조건식은 결과값이 반드시 정수(문자)이거나 문자열이어야 하며, case문의 값은 중복되면 안된다.

[Switch문 동작 과정]
1. 조건식을 계산한다.
2. 조건식의 결과와 일치하는 case문으로 이동한다.
3. 해당하는 문장을 수행한다.
4. break문을 만나면 switch문을 빠져나간다. -> 끝 

break문은 각 case문의 영역을 구분하는 역할을 한다.  
만약 break문을 생락하면 다른 break문을 만나거나 switch문의 끝을 만날 때까지 수행된다.  
case문은 여러 경우를 동시에 처리할 때 break문을 생략해 사용하기도 한다.

만약 조건식의 결과와 일치하는 case문이 없으면 default문으로 이동하게 된다. (if문의 else블럭과 같은 역할이다.)


# 반복문(iteration statements)
반복문이란 프로그램 내에서 똑같은 명령을 일정 횟수만큼 반복하여 수행하도록 제어하는 명령문이다.

프로그램이 처리하는 대부분의 코드는 반복적인 형태가 많으므로, 가장 많이 사용되는 제어문 중 하나이다.
자바에서 사용되는 대표적인 반복문의 형태는 다음과 같다.
1. while 문
2. do / while 문
3. for 문

## for문  

```java
for (초기화식;  조건식; 증감식) {
    실행문;
}
```

반복 횟수 알고 있을 때 사용
```java
int sum = 0;
for (int i=1; i<=100; i++) {
    sum += i;
}

System.out.println("1~100의 합 :" + sum);
```

초기화식 : 반복의 시작 지점 지정

조건식 : 언제까지 반복하는가

증감식 : 얼만큼 증가하는가 

## while문   

~~~java
while(조건식) { 
    실행문;
}
~~~
조건에 따라 반복할 때 사용

true일 경우 반복, false일 경우 종료

조건식에는 주로 비교 연산식, 논리 연산식 사용

```java
int i = 1;
int sum = 0;
while (i<=100) {
    sum += i;
    i++;
}

System.out.println("1~100의 합 :" + sum);
```

## do-while문

```java
do {
    실행문;
} while (조건식);  
// 조건식이 뒤로 오며 조건식이 true일 경우 반복, false일 경우 탈출한다.
```

while문과 유사하나 조건을 나중에 검사한다.

블록 내부 실행문을 우선 실행하고 그 결과에 따라 반복 실행 여부를 결정한다.(무조건 한번은 실행함)


```java
int i = 1;
int sum = 0;
do {
    sum += i;
    i++;
} while (i<=100);

System.out.println("1~100의 합 :" + sum);
```

## break문

for, while, do-while, switch문의 실행을 중지할 때 사용

주로 if문과 함께 사용
```java
int i = 1;
int sum = 0;
while (i < 100) {
    sum += i;
    i++;
    if ( i == 6) break;
}
```

## label
자바 예약어로 두개 이상의 for나 while 루프가 있을 때 사용된다.

```java
exit;
for (int i =0; i < 10; i++) {
    for (int j = 0; j < 10; j++) {
        if ( j == 6) break exit;
        }
}
```

바깥쪽 루프의 시작점으로 이동하려고 할 때 label을 사용, 실제 서비스에서는 거의 사용되지 않는다.

## continue
for, while, do-while문에서 사용되며, for문의 증감 식이나 while, do-while문의 조건식으로 이동  
if문과 함께 사용된다.
```java
for (int i =0; i < 10; i++) {
    if(i%2 !=0) { 
        continue; // i가 홀수이면 출력하지 말고 증감식으로 돌아가게 만듬.
    }
    System.out.println(i);
}
```
