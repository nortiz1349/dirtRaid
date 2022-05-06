---
title: "[JAVA STUDY] 연산자"
excerpt: 다양한 연산자를 학습해보자
excerpt_separator: "<!--more-->"
categories:
  - JavaStudy
tags:
  - JAVA
  - operator
  - instanceof
  - Study


toc: true
toc_sticky: true
date: 2022-05-03
---

## 목표

자바가 제공하는 다양한 연산자를 학습해보자.

## 학습할 것

- 산술 연산자
- 비트 연산자
- 관계 연산자
- 논리 연산자
- instanceof
- assignment(=) operator
- 화살표(->) 연산자
- 3항 연산자
- 연산자 우선 순위
- (optional) Java 13. switch 연산자

---

- 연산 (operations) : 프로그램에서 데이터를 처리하여 결과를 산출하는 것  
- 연산자 (operator) : 연산에 사용되는 표시나 기호  
- 피연산자 (operand) : 연산의 대상이 되는 데이터  
- 연산식 (expressions) : 연산자와 피연산자로 연산의 과정을 기술한 것

|연산자 종류|연산자|피연산자 수|결과값|설명|
|---|---|---|---|---|
|산술|+, -, *, /, %|이항|숫자|사칙연산 및 나머지 계산|
|부호|+, -|단항|숫자|음수와 양수의 부호|
|문자열|+|이항|문자열|두 문자열을 연결|
|대입|=, +=, -=, *=, /=, %=, &=,^=, &#124;= <<=, >>=, >>>=|이항|다양|우변의 값을 좌변의 변수에 대입|
|증감|++, --|단항|숫자|1만큼 증가/감소|
|비교(관계)|==, !=, >, <, >=, <=, instanceof|이항|boolean|값의 비교|
|논리|!, &, &#124;, &&, &#124;&#124;|단항, 이항|boolean|논리적 NOT, AND, OR 연산|
|조건|(조건식) ? A : B|삼항|다양|조건식에 따라 A 또는 B 중 하나를 선택|
|비트|~, &, &#124;, ^|단항|이항|숫자, boolean|비트 NOT, AND, OR, XOR 연산|
|쉬프트|>>, <<, >>>|이항|숫자|비트를 좌측/우측으로 밀어서 이동|

>:bell: 마크다운 문법에서 테이블 스플리터인 `|` 버티컬 바를 테이블 안에 쓰려면 `&#124;` 로 대신 입력하면 된다.

## 산술 연산자 (Mathematical Operators)

산술 연산자는 대부분의 프로그래밍에서 공통적으로 사용하는 연산이다. 더하기 (+),  빼기 (-), 곱하기 (*), 나누기 (/)와 추가로 나머지(%) 이렇게 5개이다. 나머지는 정수를 나눈 나머지 값이고, 정수 나눗셈의 결과는 버림으로 처리한다.

참고로 축약된 표현식도 가능한데, 대표적 예로 `x += 4` 와 같은 표현식이다. 이 표현식은 대입(할당)과 산술 연산을 동시에 할 수 있는 표현식이다.

### :bell: 주의사항
  
- 연산 과정에서 타입 캐스팅 혹은 타입 프로모션이 발생할 수 있기 때문에 연산을 할 때 주의가 필요하다. (원하지 않는 결과가 발생할 수 있음)  

  ```java
  int v1 = 5;
  int v2 = 2;
  byte result1 = v1 + v2;
  // 정수와 정수의 연산 결과는 정수이므로 int 타입으로 결과 값이 저장된다.
  int result2 = v1 / v2;
  // 결과는 2.5 이지만 소수점 버림되어 int 타입 2 가 저장된다.
  int result3 = (double) v1 / v2;
  // 타입캐스팅을 통해 2.5를 얻을 수 있다.
  ```

- 결과 값 산출시 Overflow 에 주의한다.  
  
  ```java
  int x = 1_000_000;
  int y = 1_000_000;
  int z = x * y;    
  // 1_000_000_000_000, int타입에 수용 가능한 숫자는 약 21억까지다.
  // 에러가 발생하는 것이 아니라 쓰레기 값이 나온다.
  ```

- 정확한 계산은 정수를 사용하는 것이 좋다.

  ```java
  int x = 1;
  double y = 0.1;
  int z = 7;

  double result = x - (y * z);
  // 1 - (0.1 * 7) = 0.3
  // 실제 result 출력은 0.2999999.... 
  ```

  이진 포맷의 가수를 사용하는 부동소수점 타입(float, double)은 0.1을 정확히 표현할 수 없어 근사치로 처리하기 때문이다.

- NaN, Infinity 연산에 주의한다. (우측 피연산자가 0인 경우)  
  
  |연산자|결과|결과 검사 메서드|
  |---|---|---|
  |`/`|Infinity|`Double.isInfinite()`|
  |`%`|NaN|`Double.isNaN()`|

  ```java
  public class Main {
  
  public static void main(String[] args) {
    int a = 5;
    double b = 0.0;
    
    double c = a / b;
    double d = a % b;
    
    System.out.println("Infinite 확인 : " + Double.isInfinite(c));
    System.out.println("NaN 확인 : " + Double.isNaN(d));

    // Infinite 및 NaN 연산 확인
    System.out.println("c+2 = " + (c+2));
    System.out.println("d+2 = " + (d+2));
    
    // 연산 수행 조건문 사용
    if(Double.isInfinite(c) || Double.isNaN(c)) {
      System.out.println("값 산출 불가");
      } 
      else {
        System.out.println(c+2);
    }
  }
  }
  /* Infinite 확인 : true
   * NaN 확인 : true
   * c+2 = Infinity
   * d+2 = NaN
   * 값 산출 불가
   */
  ```

## 비트 연산자

## 관계 연산자

## 논리 연산자

## instanceof

## assignment(=) operator

## 화살표(->) 연산자

## 3항 연산자

## 연산자 우선 순위

## (optional) Java 13. switch 연산자
