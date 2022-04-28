---
title: "JAVA 클래스와 객체 (1)"
excerpt: object, class, method, instance, constructor
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - java
  - eclipse
  - Do it!

toc: true
toc_sticky: true
date: 2022-04-22
---

## 객체 지향 프로그래밍

## 클래스와 객체 1

### 1) 클래스

객체 지향 프로그래밍은 클래스를 기반으로 프로그래밍 한다.  
클래스는 객체의 속성과 기능을 코드로 구현한 것이고, 객체를 클래스로 구현하는 것을 '클래스를 정의한다'라고 하는데, 클래스 이름과 속성, 특성이 필요하다.

클래스 속성을 '멤버 변수'라고 하고 클래스 내부에 선언 한다.

```java
// (접근 제어자) class 클래스이름 {
//    멤버 변수;
//    메서드;
//  }

// 학생 클래스 만들기
package classpart;

public class Student {  // 클래스 이름은 대문자로 시작한다.
  int studentID;
  String studentName;
  int grade;
  String adrees;
}
// Student라는 클래스를 만들고 학생이라는 객체의 속성을 변수로 선언
```

멤버변수는 기본자료형`(int, double, ...)`, 참조자료형`(String, 개발자가 만든 클래스가 다른 클래스에서 사용하는 멤버 변수의 자료형, ...)`이 될 수 있다.

변수 선언 후 클래스에서 객체가 가지는 속성을 사용해 기능을 구현할 수 있다. 이 기능을 멤버함수 혹은 `메서드(method)`라고 한다.

- 패키지  
클래스 파일의 묶음으로 다양한 계층으로 구성하여 클래스를 관리할 수 있다.  
`package [package_name];`  
패키지가 다르면 같은 이름의 클래스라고 할지라도 서로 연관이 없다.

### 2) 매서드

메서드는 함수(function)의 한 종류.

- 함수 정의하기

  ```java
  int add (int num1, int num2) {  // 함수이름 add, 매개변수 num1, num2
    int result;
    result = num1 + num2  // 
    return result;
  }
  ```

- 함수 구현하고 호출하기  
`return` : 결과 값을 반환함

  ```java
  package classpart;
  public class FunctionTest {
  public static void main(String[] args) {
    int num1 = 10;
    int num2 = 20;
    
    int sum = add(num1, num2); // add 함수 호출하여 sum에 넘김
    System.out.println(num1 + " + " + num2 + " = " + sum + "입니다");
    }

    public static int add(int n1, int n2){
    int result = n1 + n2;
    return result; // 결과 값 반환
    }
  }
  ```

- 함수 호출과 스택 메모리  
함수를 호출하면 그 함수만을 위한 메모리 공간이 할당 된다. 이 메모리 공간을 `stack`이라고 한다. `LIFO` 구조로 되어 있다.

### 3) 클래스와 인스턴스

### 4) 생성자

### 5) 참조 자료형

### 6) 정보 은닉
