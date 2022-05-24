---
title: "JAVA 클래스와 객체 (2)"
excerpt: this, static
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - object
  - class

toc: true
toc_sticky: true
date: 2022-04-27
---

## `this`

### 생성자 `this()`

- 생성자에서 다른 생성자 호출할 때 사용  
- 다른 생성자 호출시 첫 줄에서만 사용가능

코드의 중복을 제거하기 위해 클래스 내부에서 생성자를 서로 호출해야 하는 경우가 빈번한데, 그 때 클래스 이름으로 호출하는 것이 아니라 `this`로 호출한다.

### 참조변수 `this`

- 인스턴스 자신을 가리키는 참조변수
- 생성자 this()와는 전혀 완전히 다른 것이다. 분리해서 생각하자.
- 생성자와 인스턴스 메서드에서 사용하고 클래스 매서드에서는 사용 불가하다.
- 지역변수와 인스턴스 변수의 이름이 같을 때 서로 구별하기 위해 사용한다.

인스턴스의 주소가 저장되어 있고 모든 인스턴스 메서드에 지역변수로 숨겨진 채로 존재한다. 즉 선언을 하지 않고도 사용할 수 있다.

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

### 객체 간 협력

```java
// 학생 클래스
// 멤버변수 : 이름,학년,가진돈
// 메서드 : 버스를 탄다. 지하철을탄다. 학생의 현재 정보를 보여준다.
package cooperation;
public class Student {
 public String studentName;
 public int grade;
 public int money;
 // 학생 이름과 가진 돈을 매개변수로 받는 생성자
 public Student(String studentname, int money) {
  this.studentName = studentname;
  this.money = money;
 }
 // 학생이 버스를 타면 1,000원을 지불하는 기능을 구현한 메서드
 public void takeBus(Bus bus) {
  bus.take(1000);
  this.money -= 1000;
 }
 // 지하철을 타면 1,500원을 지불하는 메서드
 public void takeSubway(Subway subway) {
  subway.take(1500);
  this.money -= 1500;
 }
 
 public void showInfo() {
  System.out.println(studentName + " 남은돈 " + money);
 }
}
```

```java
// 버스 클래스
package cooperation;
public class Bus {
 int busNumber;
 int passengerCount;
 int money;
 // 버스 번호를 매개 변수로 받는 생성자
 public Bus(int busNumber) {
  this.busNumber = busNumber;
 }
 // 승객이 버스에 탄 경우를 구현한 메서드
 public void take(int money) {
  this.money += money;
  passengerCount++;
 }
 public void showInfo() {
  System.out.println("버스승객 " + passengerCount + " 수입 " + money);
 }
}
```

```java
// 지하철 클래스
package cooperation;
public class Subway {
 int lineNumber;
 int passengerCount;
 int money;
 // 지하철 호선을 매개 변수로 받는 생성자
 public Subway(int lineNumber) {
  this.lineNumber = lineNumber;
 }
 // 승객이 버스에 탄 경우를 구현한 메서드
 public void take(int money) {
  this.money += money;
  passengerCount++;
 }
 public void showInfo() {
  System.out.println("지하철승객 " + passengerCount + " 수입 " + money);
 }
}
```

```java
// 버스, 지하철 타기 클래스
package cooperation;
public class TakeTrans {
 public static void main(String[] args) {
  Student parkStudent = new Student("박종운", 20000); // 학생 두명 생성
  Student kangStudent = new Student("강주하", 50000);
  
  Bus bus115 = new Bus(115);   // 버스 생성
  parkStudent.takeBus(bus115); // 버스 탐
  parkStudent.showInfo();      // 학생 정보 출력
  bus115.showInfo();
  
  Subway subwayLine2 = new Subway(2); // 지하철 생성
  kangStudent.takeSubway(subwayLine2);
  kangStudent.showInfo();
  subwayLine2.showInfo();
 }
}
```

## `static`  

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

### 1. 클래스변수

`static`으로 선언된 변수, 클래스 이름으로 참조하여 사용할 수도 있다.

```java
...
System.out.println(Student1.serialNum);
// static으로 선언된 serialNum 변수를 클래스 이름으로 참조
...
```

### 2. 클래스 메서드

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

### 3. 클래스 메서드와 인스턴스 변수

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

## 변수 유효 범위

|변수유형|선언위치|사용범위|메모리|생성과소멸|
|:---:|:---:|:---:|:---:|:---:|
|지역 변수(로컬변수)|함수 내부에 선언|함수 내부에서만 사용|스택|함수가 호출될 때 생성되고 함수가 끝나면 소멸함|
|멤버 변수(인스턴스변수)|클래스 멤버 변수로 선언|클래스 내무에서 사용하고 private이 아니면 참조 변수로 다른 클래스에서 사용 가능|힙|인스턴스가 생성될 때 힙에 생성되고, 가비지 컬렉터가 메모리를 수거할 때 소멸됨|
|static 변수(클래스 변수)|static 예약어를 사용하여 클래스 내부에 선언|클래스 내부에서 사용하고 private이 아니면 클래스 이름으로 다른 클래스에서 사용 가능|데이터 영역|프로그램이 처음 시작할 때 상수와 함께 데이터 영역에 생성되고 프로그램이 끝나고 메모리를 해제할 때 소멸됨|

### `static` 응용 -  싱글톤 패턴

상황에 따라 여러개의 인스턴스가 필요한 경우, 단 하나의 인스턴스가 필요한 경우도 있다. 단 하나의 인스턴스를 생성하는 디자인 패턴을 싱글톤 패턴이라고 한다. 디자인 패턴이란 프로그램 특성에 따른 설계 유형을 이론화한 내용이고 내용이 방대하므로 별도로 스터디가 필요하다.

## 연습문제

1. 클래스 내부에서 자신의 주소를 가리키는 예약어를 `this`라고 합니다.
2. 클래스에 여러 생성자가 오버로드되어 있을 경우에 하나의 생성자에서 다른 생성자를 호출할 때 `this`를 사용합니다.
3. 클래스 내부에 선언하는 `static`변수는 생성되는 인스턴스 마다 만들어지는 것이 아닌 여러 인스턴스가 공유하는 변수입니다. 따라서 클래스에 기반한 유일한 변수라는 의미로 `클래스 변수`라고도 합니다.
4. 지역 변수는 함수나 메서드 내부에서만 사용할 수 있고 `스택` 메모리에 생성됩니다. 멤버 변수 중 `static` 예약어를 사용하는 static `데이터영역`메모리에 생성됩니다.
