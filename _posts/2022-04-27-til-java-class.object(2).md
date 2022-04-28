---
title: "JAVA 클래스와 객체 (2)"
excerpt: this, static
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - java
  - eclipse
  - Do it!

toc: true
toc_sticky: true
date: 2022-04-27
---

## 클래스와 객체 2

### 1. `this`

생성된 인스턴스 스스로를 가리키는 예약어

```java
class BirthDay {
  int day;
  int month;
  int year;
  
  public void setYear(int year) {
  this.year = year        // bDay.year = year;와 같다.
  }
}
public class ThisExample {
  public static void main (String[] args) {
    BirthDay bDay = new BirthDay();
    bDay.setYear(2000);
  }
}
// this.year = year; 문을 통해 동적 메모리에 생성된 인스턴스의 year 변수 위치를 가리키고 그 위치에 매개변수 값을 넣어준다.
```

```java
// 다른 생성자를 호출하는 this
class Person {
  String name;
  int age;

  Person () {
    this("이름없음", 1);  // this를 사용해 Person(String, int) 생성자 호출
  }
  Person (String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```

#### 객체 간 협력

예제 코딩 연습

### 2. `static`  

`static` 변수는 클래스 내부에 선언하지만, 다른 멤버 변수처럼 인스턴스가 생성될 때마다 새로 생성되는 변수가 아니다. `static` 변수는 프로그램이 실행되어 메모리에 올라갔을 때 딱 한 번 메모리 공간이 할당된다. 그리고 그 값은 모든 인스턴스가 공유한다.

```java
// 학번 자동 부여
public class Student1 {
  public static int serialNum = 1000;
  public int studentID;

  public Student1() {         // 생성자
    serialNum++;              // Student1 인스턴스가 생성될때마다 증가
    studentID += serialNum;   // 증가된 값을 학번 인스턴스 변수에 
  }
}
```

- 인스턴스 간 공통으로 사용할 값이 필요한 경우 유용하다.

#### - 클래스변수

`static`으로 선언된 변수, 클래스 이름으로 참조하여 사용할 수도 있다.

```java
...
System.out.println(Student1.serialNum);
// static으로 선언된 serialNum 변수를 클래스 이름으로 참조
...
```

#### - 클래스 메서드

```java
public class Student2 {
  private static int serialNum = 1000; // private 변수로 변경
  int studentID;

  ...

  public static int getSerialNum() {  // serialNum의 get() 메서드
    int i = 0;
    return serialNum;
  }
  public static void setSerialNum(int serialNum) {  // serialNum의 set() 메서드
    Student2.serialNum = serialNum;
  }
}
// 외부 클래스에서 serialNum 값을 사용하려면 get() 메서드 호출
// serialNum 값을 변경하려면 set() 메서드를 사용해야 함.
```

```java
...
// serialNum 값을 가져오기 위해 get() 메서드를 클래스 이름으로 직접 호출
System.out.println(Student2.getSerialNum() );
...
```

#### - 클래스 메서드와 인스턴스 변수

클래스 메서드 내부에서는 인스턴스 변수를 사용할 수 없다.

```java
public class Student2 {
  private static int serialNum = 1000;
  String studentName; // 인스턴스 변수임.
  ...
  public static int getSerialNum() {
    int i = 10; // 클래스 메서드 내부에서 선언한 변수는 지역변수로써 getSerialNum() 내부에서만 사용 가능하고 메서드가 끝나면 사라지는 변수이다.
    studentName = "이지원"; // 오류발생 - studentName은 Student2 클래스의 멤버변수로써 인스턴스가 생성될때 만들어지는 인스턴스 변수이므로 클래스 메서드 내부에서는 사용할 수 없다.
    return serialNum;
  }
}
 - int i = 10
```

### 변수 유효 범위

|변수유형|선언위치|사용범위|메모리|생성과소멸|
|:---:|:---:|:---:|:---:|:---:|
|지역 변수(로컬변수)|함수 내부에 선언|함수 내부에서만 사용|스택|함수가 호출될 때 생성되고 함수가 끝나면 소멸함|
|멤버 변수(인스턴스변수)|클래스 멤버 변수로 선언|클래스 내무에서 사용하고 private이 아니면 참조 변수로 다른 클래스에서 사용 가능|힙|인스턴스가 생성될 때 힙에 생성되고, 가비지 컬렉터가 메모리를 수거할 때 소멸됨|
|static 변수(클래스 변수)|static 예약어를 사용하여 클래스 내부에 선언|클래스 내부에서 사용하고 private이 아니면 클래스 이름으로 다른 클래스에서 사용 가능|데이터 영역|프로그램이 처음 시작할 때 상수와 함께 데이터 영역에 생성되고 프로그램이 끝나고 메모리를 해제할 때 소멸됨|

### `static` 응용 -  싱글톤 패턴
