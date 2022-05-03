---
title: "JAVA 시작, 변수와 자료형, 여러가지 연산자"
excerpt: Variable, Data Type, Operator
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - java
  - eclipse
  - Do it!
  - variables
  - operator
  - type

toc: true
toc_sticky: true
date: 2022-04-21
---

## 1. 자바 프로그래밍 시작하기

### 자바 특징

1. 플랫폼에 영향을 받지 않으므로 다양한 환경에서 사용할 수 있다.
2. 객체 지향 언어이기 때문에 유지보수가 쉽고 확장성이 좋다.
3. 프로그램이 안정적이다.
4. 오픈 소스이다.

### 자바로 할 수 있는 일?

1. 웹서버(우리가 할 것)
2. 안드로이드앱
3. 게임(마인크래프트)

---

## 2. 변수와 자료형

### 1) 변수

자바스크립트에서는 변수를 선언하고 무엇을 담을지에 대해서는 정하지 않았지만, 자바에서는 변수를 선언 할 때 자료형이 무엇인지 명시한다.

- 변수 선언하고 값 대입하기

  ```java
  package chapter2;

  public class Variable1 {

  public static void main(String[] args) {
    int level; // 정수형 변수 level을 선언
    level = 10; // 값 10을 level 변수에 대입
    System.out.println(level); // level 값 출력
  }

  }
  // 선언함과 동시에 대입 가능
  // int level = 10;
  ```

변수 이름에 예약어는 사용할 수 없다. 영문자나 숫자를 사용할 수 있지만 숫자로 시작할 수는 없다. 특수문자는 `$`, `_` 만 사용 가능하다.

### 2) 정수 자료형

`byte` < `short` < `int` < `long`

### 3) 문자 자료형

`char`

  ```java
  package chapter2;

  public class CharacterEx1 {

  public static void main(String[] args) {
    char ch1 = 'A';
    System.out.println(ch1);  // 문자출력
    System.out.println((int)ch1); // 문자에 해당하는 정수 값(아스키코드) 출력
    
    char ch2 = 66;     // 정수 값 대입
    System.out.println(ch2);  // 정수 값에 해당하는 문자 출력
    
    int ch3 = 67;
    System.out.println(ch3);  // 문자 정수 값 출력
    System.out.println((char)ch3); // 정수 값에 해당하는 문자 출력
  }

  }
  ```

### 4) 실수 자료형

`float` < `double`

아주 정밀한 계산이 필요한 경우에 주로 사용한다. `double`이 기본형.

### 5) 논리 자료형

`boolean`

### 6) 자료형 없이 변수 선언

`var`

자바10부터 생긴 문법, 자바 수업에서는 사용하지 않을 예정.

### 7) 상수와 리터럴

#### 상수 선언

`final`

프로그램 내부에서 반복적으로 사용하고, 변하지 않아야 하는 값을 상수로 선언하여 사용한다.

  ```java
  final double PI = 3.14;
  final int MAX_NUM = 100;
  ```

상수 이름은 `대문자`를 사용하고 연결기호 `_` 를 활용한다.

#### 리터럴

프로그램에서 사용하는 모든 숫자, 문자, 논리값을 일컫는 말. 간단히 말해서 변수에 대입 하기전의 값. 프로그램이 시작할 때 시스템에 같이 로딩되어 특정 메모리 공간인 상수 풀에 놓인다.

### 8) 형 변환

정수와 실수는 컴퓨터 내부에서 표현되는 방식이 전혀 다르다. 따라서 두가지 형의 연산을 그대로 수행할 수 없고 형변환이 이루어진 뒤 연산이 진행된다.

#### 묵시적 형 변환 (자동 형 변환)

1. 바이트 크기가 작은 자료형에서 큰 자료형으로 대입하는 경우
2. 덜 정밀한 자료형에서 더 정밀한 자료형으로 대입하는 경우
3. 연산중에 자동 변환되는 경우

#### 명시적 형 변환 (강제 형 변환)

자동 형 변환의 반대의 경우로 생각하면 된다. 큰 자료형에서 작은 자료형으로 대입하면 값이 손실될 수 있는 위험이 있으므로 명시적으로 형 변환을 한다.

  ```java
  int iNum = 1000;
  byte bNum = (byte)iNum; // 강제로 형을 바꾸려면 바꿀 형을 괄호를 써서 명시해야 함
  ```

위의 경우 `1000`을 `1 byte` 안에 표현할 수 없으므로 자료 손실이 발생한다.

---

## 3. 자바의 여러 가지 연산자

### 1) 기본 연산자

#### 항과 연산자

- 연산자 우선순위 : `단항 연산자` > `이항 연산자` > `삼항 연산자`

1. 대입 연산자  
`=`

2. 부호 연산자  
`+`, `-`

3. 산술 연산자  
`+`, `-`, `*`, `/`, `%`

4. 증가, 감소 연산자  
`++`, `--`  

5. 관계 연산자  
`>`, `<`, `>=`, `<=`, `==`, `!=`

6. 논리 연산자  
  `&&` : `논리 곱` : 둘다 참이면 참, 아니면 모두 거짓  
  `||` : `논리 합` : 둘 중 하나만 참이면 참, 둘 다 거짓이면 거짓  
  `!` : `부정` : 참이면 거짓으로, 거짓이면 참으로  

7. 복합 대입 연산자  
`+=`, `-=`, `*=`, `/=`, `%=`

8. 조건 연산자

    ```java
    int num = (5 > 3) ? 10 : 20;
    // 조건식 ? 결과1 : 결과2;
    // 조건식이 참이면 결과1, 거짓이면 결과2
    ```

### 2) 비트 연산자

'암호화' 작업처럼 임의의 숫자를 만들거나, 어떤 변수의 특정 비트를 꺼내보는(마스킹) 경우에 사용. 혹은 임베디드에서 연산속도를 높이기 위해, 프로그램에서 특정 값을 만들거나 연산할 때 사용한다.

1. 비트 논리 연산자  
`&`, `|`, `^`, `~`

2. 비트 이동 연산자  
`<<`, `>>`, `>>>`

### 3) 연산자 우선 순위

가독성을 위해 소괄호를 활용해서 우선 순위를 지정하는 것을 추천하며, 필요한 경우 우선 순위를 찾아서 활용하면 된다.

---

## 교제 코딩 연습

```java
// 산술 연산자를 사용하여 총점과 평균 구하기
package operator;
public class OperationEx1 {
 public static void main(String[] args) {
  int mathScore = 90;
  int engScore = 70;
  int korScore = 100;
  
  int totalScore = mathScore + engScore + korScore;
  System.out.println(totalScore);
  
  double avgScore = totalScore / 2.0;
  System.out.println(avgScore);
 }
}
```

```java
// 증가,감소 연산자를 사용하여 값 연산하기
package operator;
public class OperationEx2 {
 public static void main(String[] args) {
  int gameScore = 150;
  
  int lastScore1 = gameScore++;
  System.out.println(lastScore1); // 150
  
  int lastScore2 = gameScore--;
  System.out.println(lastScore2); // 151
 }
}
```

```java
// 단락 회로 평가
package operator;
public class OperationEx3 {
 public static void main(String[] args) {
  int num1 = 10;
  int i = 2;
  
  boolean value = ((num1 = num1 + 10) < 10) && ((i = i + 2) < 10);
  System.out.println(value); // false
  System.out.println(num1); // 20
  System.out.println(i);  // 2
  
  value = ((num1 = num1 + 10) > 10) || ((i = i + 2) < 10);
  System.out.println(value); // true
  System.out.println(num1); // 30
  System.out.println(i);  // 2
 }
}
```

```java
// 조건 연산자를 사용하여 부모님의 나이 비교하기
package operator;
public class OperationEx4 {
 public static void main(String[] args) {
  int fatherAge = 45;
  int motherAge = 47;
  
  char ch;
  ch = (fatherAge > motherAge) ? 'T' : 'F';
  
  System.out.println(ch);
  
  // num이 짝수면 true, 홀수면 false
  int num = 10;
  boolean isEven;
  isEven =  (num % 2) == 0 ? true : false;
  System.out.println(isEven);
 }
}
```

```java
// p.88 연습문제
package chapter3;
public class OperationEx1 {
 public static void main(String[] args) {
  int myAge = 23;
  int teacherAge = 38;
  
  boolean value = (myAge > 25);
  System.out.println(value); // false
  
  System.out.println(myAge <= 25); // true
  System.out.println(myAge == teacherAge); // false
  
  char ch;
  ch = (myAge > teacherAge) ? 'T' : 'F';
  System.out.println(ch); // F
  
  // q2, 3
  int num;
  num = -5 + 3 * 10 / 2;
  System.out.println(num);  // 10
  System.out.println(num++);  // 10
  System.out.println(num);  // 11
  System.out.println(--num);  // 10
  
  // q4
  int num1 = 10;
  int num2 = 20;
  boolean result;
  
  result = ((num1 > 10) && (num2 > 10));
  System.out.println(result); // false
  result = ((num1 > 10) || (num2 > 10));
  System.out.println(result); // true
  
  // q7
  int result2 = (num >= 10) ? num2 + 10 : num2 -10;
  System.out.println(result2); // 30
  
  // q6
  num = 8;
  System.out.println(num += 10); // 18
  System.out.println(num -= 10); // 8
  System.out.println(num >>= 10); // 0
  }
}
```
