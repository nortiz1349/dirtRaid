---
title: "JAVA 제어 흐름 이해하기"
excerpt: if else, switch, while, for
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - statement

toc: true
toc_sticky: true
date: 2022-04-22
---

## 4. 제어 흐름 이해하기

### 조건문

#### 1) `if`, `if-else`

```java
if (조건식) {
  수행문1;  // 조건 참 > 수행문1
} 
else {
  수행문2;  // 조건 거짓 > 수행문2
}
```

#### 2) `if - else if - else`

```java
if (조건식1) {
  수행문1;  // 조건1 참 > 수행문1
}
else if (조건식2) {
  수행문2;  // 조건2 참 > 수행문2
}
else if (조건식3) {
  수행문3;  // 조건3 참 > 수행문3
}
else {
  수행문4;  // 조건 1,2,3 거짓 > 수행문4
}
수행문5;
```

#### 3) `switch-case`

조건식의 결과가 정수 또는 문자열 값이고 그 값에 따라 수행되는 경우가 각각 다른 경우에는 `switch-case`문으로 구성하는게 깔끔하고 가독성도 좋다.
`if-else`문의 `else`와 `switch-case`문의 `defalut`가 같은 역할을 한다.

```java
switch(조건) {
  case 값1 : 수행문1;
    break;
  case 값2 : 수행문2;
    break;
  case 값3 : 수행문3;
    break;
  defalut : 수행문4;  
}
```

### 반복문

#### 1) `while`

```java
while(조건식) { // 조건 참 > 수행문1 반복수행
  수행문1;
  ...
}             
  수행문2;    // 조건 거짓 > 수행문2
```

#### 2) `do-while`

```java
do {
  수행문1;
  ...
} while(조건식); // 수행문1 > 조건 참 > 수행문1
  수행문2;       // 수행문1 > 조건 거짓 > 수행문2
  ...
```

#### 3) `for`

반복문 중에서 가장 많이 사용함.

```java
for(초기화식; 조건식; 증감식) {
  수행문;
}
// 초기화식은 for문이 시작할 때 한 번 수행. 사용할 변수를 초기화 함.
```

for문 요소(초기화식, 조건식, 증감식)는 상황에 따라 생략 가능하다.

- 반복 횟수가 정해진 경우 `for`문
- 수행문을 반드시 한 번 이상 수행해야 하는 경우 `do-while`
- 참, 거짓에 따라 반복문 수행하는 경우 `while`

#### 4) `continue`

특정 조건에서 반복문을 수행하지 않고 건너뛰어야 할 때 사용.

```java
...
for(num = 1; num <= 100; num++) { // 100까지 반복
  if(num % 2 == 0)                // num 값이 짝수인 경우
    continue;                     // 이후 수행을 생략하고 num++ 수행
  total += num;                   // num 값이 홀수인 경우에만 수행
}
...
```

#### 5) `break`

---

## 교제 코딩 연습

```java
// 나이에 따라 다른 문장 출력하기
package ifexample;
public class ifExample1 {
 public static void main(String[] args) {
  int age = 7;
  if (age >= 8) {
   System.out.println("학교에 다닙니다.");
  }
  else {
   System.out.println("미취학 아동입니다.");
  }
  
  // female, male
  char gender = 'F';
  if (gender == 'F') {
   System.out.println("female");
  }
  else {
   System.out.println("male");
  }  
 }
}
```

```java
// if-else if-else문으로 입장료 계산하기
package ifexample;
public class IfExample2 {
 public static void main(String[] args) {
  int age = 60;
  int charge;
  
  if (age < 8) {
   charge = 1000;
   System.out.println("미취학 아동입니다.");
  }
  else if (age < 14) {
   charge = 2000;
   System.out.println("초등학생입니다.");
  }
  else if (age < 20) {
   charge = 2500;
   System.out.println("중고등학생입니다.");
  }
  else if (age >= 60) {
   charge = 0;
   System.out.println("경로우대입니다.");
  }
  else {
   charge = 3000;
   System.out.println("일반인입니다.");
  }
  System.out.println("입장료는 " + charge + "원 입니다.");
 }
}
```

```java
// 성적에 따라 학점 부여하기
package ifexample;
public class IfExample2_2 {
 public static void main(String[] args) {
  int score = 87;
  char grade;
  
  if (score > 90 ) {
   grade = 'A';
  }
  else if (score > 80) {
   grade = 'B';
  }
  else if (score > 70) {
   grade = 'C';
  }
  else if (score > 60) {
   grade = 'D';
  }
  else {
   grade = 'F';
  }
  System.out.println("성적은 " + grade + " 입니다.");
 }
}
```

```java
// switch-case example
package ifexample;
public class SwitchCase {
 public static void main(String[] args) {
  int ranking = 1;
  char medalColor;
  
  switch (ranking) {
  case 1: medalColor = 'G';
   break;
  case 2: medalColor = 'S';
   break;
  case 3: medalColor = 'B';
   break;
  default:
    medalColor = 'A';
  }
  System.out.println(ranking + "등의 메달 색깔은 " + medalColor + " 입니다.");
 }
}
```

```java
// switch-case example 2
package ifexample;
public class SwitchCase2 {
 public static void main(String[] args) {
  String medal = "Gold";
  
  switch (medal) {
  case "Gold":
   System.out.println("금메달입니다.");
   break;
  case "Silver":
   System.out.println("은메달입니다.");
   break;
  case "Bronze":
   System.out.println("동메달입니다.");
   break;
  default:
   System.out.println("메달이 없습니다.");
   break;
  }
 }
}
```

```java
// self coding p.106
package ifexample;
public class SwitchCase2_2 {
 public static void main(String[] args) {
  int floor = 3;
  
  switch (floor) {
  case 1:
   System.out.println("약국입니다.");
   break;
  case 2:
   System.out.println("정형외과입니다.");
   break;
  case 3:
   System.out.println("피부과입니다.");
   break;
  case 4:
   System.out.println("치과입니다.");
   break;
  case 5:
   System.out.println("헬스클럽입니다.");
   break;

  default:
   break;
  }
 }
}
```

```java
// while문 활용하여 1부터 10까지 더하기
package loopexample;
public class WhileExample1 {
 public static void main(String[] args) {
  int num = 1;
  int sum = 0;
  
  while (num <= 10) {
   sum += num;
   num++;
  }
  System.out.println("1부터 10까지의 합은 " + sum + "입니다.");
  
  // 50까지 더하기
  num = 1;
  sum = 0;
  
  while (num <= 50) {
   sum += num;
   num++;
  }
  System.out.println("1부터 50까지의 합은 " + sum + "입니다.");
 }
}
```

```java
package loopexample;
public class ForExample1 {
 public static void main(String[] args) {
  int i;
  int sum;
  
  for(i = 1, sum = 0; i<=10; i++) {
   sum += i;
  }
  System.out.println(sum);
  
  // 안녕하세요1~안녕하세요10
  for(i = 1; i <= 10; i++) {
   System.out.println("안녕하세요" + i);
  }
 }
}
```

```java
// 구구단
package loopexample;
public class NestedLoop {
 public static void main(String[] args) {
  int dan;
  int times;
  
  for (dan=2; dan <= 9; dan++) {
   for (times = 1; times <= 9; times++) {
    System.out.println(dan + "X" + times + "=" + dan*times);
   }
   System.out.println();
  }
 }
}
```

```java
// 1~100 홀수만 더하기
package loopexample;
import java.util.EnumMap;
public class ContinueExample {

 public static void main(String[] args) {
  int i;
  int total = 0;
  
  for (i = 1; i <= 100; i++) {
   if(i % 2 == 0)
    continue;
   total += i;
  }
  System.out.println("1~100 홀수합은 " + total);
  
  // 3의 배수만 출력
  for (i = 1; i <= 100; i++) {
   if(i % 3 != 0)
    continue;
   System.out.println(i);
  }
 }
}
```

```java
package loopexample;
public class BreakExample2 {
 public static void main(String[] args) {
  int sum = 0;
  int num = 0;
  
  for (num=0; ; num++) {
   sum+=num;
   if(sum>=100)
    break; // break문을 감싸고 있는 반복문을 빠져나옴.
  }
  System.out.println(num);
  System.out.println(sum);
 }
}
```

```java
// q1. operator 값이 +,-,*,/ 인 경우 사칙연산 하는 프로그램, if-else if, swith-case문으로 작성
package Example4;
public class PracticeP123 {
 public static void main(String[] args) {
  int num1 = 10;
  int num2 = 2;
  char operator = '/';
  
  // if-else
  if (operator == '+') {
   System.out.println(num1 + "+" + num2 + "=" + (num1 + num2));
  }
  else if (operator == '-') {
   System.out.println(num1 + "-" + num2 + "=" + (num1 - num2));
  } 
  else if (operator == '*') {
   System.out.println(num1 + "*" + num2 + "=" + num1 * num2);
  } 
  else if (operator == '/') {
   System.out.println(num1 + "/" + num2 + "=" + num1 / num2);
  }
  
  // switch-case
  switch (operator) {
  case '+':
   System.out.println(num1 + "+" + num2 + "=" + (num1 + num2));
   break;
  case '-':
   System.out.println(num1 + "-" + num2 + "=" + (num1 - num2));
   break;
  case '*':
   System.out.println(num1 + "*" + num2 + "=" + (num1 * num2));
   break;
  case '/':
   System.out.println(num1 + "/" + num2 + "=" + (num1 / num2));
   break;
  }
 }
}
```

```java
// 짝수단 구구단
package Example4;
public class PracticeP123_2 {
 public static void main(String[] args) {
  int dan;
  int times;
  
  for (dan=2; dan <= 9; dan++) {
   if (dan % 2 != 0) // 짝수인 경우만 
    continue;  // 실행
   for (times = 1; times <= 9; times++) {
    System.out.println(dan + "X" + times + "=" + dan*times);
   }
   System.out.println();
  }
 }
}
```

```java
// 구구단 - 단보다 곱하는 수가 작거나 같은 경우까지만 출력
package Example4;
public class PracticeP123_3 {
 public static void main(String[] args) {
  int dan;
  int times;
  
  for (dan=2; dan <= 9; dan++) {
   for (times = 1; times <= 9; times++) {
    System.out.println(dan + "X" + times + "=" + dan*times);
    if (dan <= times) // 곱하는 수보다 작거나 같으면
     break;   // 나감
   }
   System.out.println();
  }
 }
}
```

```java
//    *  
//   ***  
//  ***** 
// *******
package Example4;
public class PracticeP123_4 {
 public static void main(String[] args) {
  int lineCount = 4;
  int starCount = 1;
  int spaceCount = lineCount / 2 + 1;
  
  for (int i = 0; i < lineCount; i++) {
   for (int j = 0; j < spaceCount; j++) {
    System.out.print(' ');
   }
   for (int j = 0; j < starCount; j++) {
    System.out.print('*');
   }
   for(int j = 0; j < spaceCount; j++) {
    System.out.print(' ');
   }
   
   spaceCount -= 1;
   starCount +=2;
   System.out.println();
  }
 }
}
```

```java
//    *  
//   ***  
//  ***** 
// *******
//  *****
//   ***
//    *
package Example4;
public class PracticeP123_5 {
 public static void main(String[] args) {
  int lineCount = 9;
  int starCount = 1;
  int spaceCount = lineCount / 2 + 1;
  
  for (int i = 0; i < lineCount; i++) {
   for (int j = 0; j < spaceCount; j++) {
    System.out.print(' ');
   }
   for (int j = 0; j < starCount; j++) {
    System.out.print('*');
   }
   for(int j = 0; j < spaceCount; j++) {
    System.out.print(' ');
   }
   if (i < lineCount/2) {
    spaceCount -= 1;
    starCount +=2;
   }
   else {
    spaceCount += 1;
    starCount -= 2;
   }
   System.out.println();
  }
 }
}
```
