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
  - object

toc: true
toc_sticky: true
date: 2022-04-22
---

## 객체 지향 프로그래밍

## 클래스와 객체 1

### 1) 클래스 (class)

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

### 2) 매서드 (method)

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
함수를 호출하면 그 함수만을 위한 메모리 공간이 할당 된다. 이 메모리 공간을 `stack`이라고 하고 `LIFO` 구조로 되어 있다. 위의 예제로 설명 하자면,  
  1. 가장 먼저 `main()` 함수를 호출하기 때문에 `main()` 함수에 포함된 변수 `num1`, `num2`, `sum`을 저장할 공간이 스택에 생성된다.
  2. 그 다음 `main()`에서 `add()` 함수를 호출하면 `add()`함수에서 사용할 변수 `n1`, `n2` 메모리 공간이 스택에 생성되고,
  3. `add()` 함수 수행이 끝나고 결과 값이 반환되면 `add()` 함수가 사용하면 메모리 공간은 사라진다.
  4. `num1`, `num2` 변수와 `n1`, `n2` 변수는 서로 다른 메모리 공간을 사용한다.
  5. 함수 내부에서만 사용하는 변수를 지역 변수라고 하고 스택 메모리에 생성된다.

### 3) 클래스와 인스턴스

`main()`함수는 JVM이 프로그램을 시작하기 위해 호출하는 함수이다. 클래스 내부에 만들어지지만 메서드는 아니다. `main()` 함수는 클래스 안에 포함할 수도 있고, 별도로 실행을 위한 클래스로 분리하여 만들기도 한다.

```java
// Student 클래스 안에 있는 main() 함수
package classpart;
public class Student {
 int studentID;
 String studentName;
 int grade;
 String adress;

 public String getStudentName() {
  return studentName;
 }
 public static void main(String[] args) {
  Student studentPark = new Student();  // 인스턴스 생성
  studentPark.studentName = "박종운";
  
  System.out.println(studentPark.studentName);
  System.out.println(studentPark.getStudentName());
 }
}
```

- main() 함수를 포함한 실행 클래스를 따로 만들기 위해 클래스를 별도로 생성한다.

```java
// 분리된 main() 함수
package classpart;
public class StudentTest {
 public static void main(String[] args) {
  Student studentPark = new Student();
  studentPark.studentName = "박종운";
  
  System.out.println(studentPark.studentName);      // 멤버변수 사용
  System.out.println(studentPark.getStudentName()); // 메서드 사용
 }
}
```

```java
package classpart;
public class Student {
 int studentID;
 String studentName;
 int grade;
 String adress;

 public String getStudentName() {
  return studentName;
 }
}
```

- 위의 두 클래스는 같은 패키지에 속해 있는 케이스이고 만약 패키지가 다르다면 `import`문을 사용해서 클래스를 불러올 수 있다.

#### 1. 인스턴스 생성하기

`클래스형 변수이름 = new 생성자;`

클래스를 사용하려면 `new` 예약어를 사용하여 `인스턴스`를 생성해야 한다. 이것은 클래스를 사용하기 위한 메모리 공간(힙 메모리)을 할당 받는다는 뜻이다.

```java
Student studenPark = new Student()
// Student 클래스 자료형으로 studentPark 변수를 선언
// new 예약어로 Student 클래스를 생성하여 studentPark에 대입한다는 뜻
// studentPark을 참조변수라고 하고, 이 변수가 생성된 인스턴스를 가리킨다.
```

#### 2. 참조변수 사용하기

참조변수를 사용하면 인스턴스의 멤버 변수와 메서드를 참조하여 사용할 수 있는데, 도트 `.` 연산자를 사용한다.

```java
  studentPark.studentName = "박종운";                 // 멤버변수 사용
  System.out.println(studentPark.getStudentName());   // 메서드 사용
```

#### 3. 인스턴스와 힙 메모리

위의 예제에서 `new Student()`를 선언하면 Student의 멤버변수인 `studentID`, `studentName` 등을 저장할 공간이 있어야 한다. 이 때 사용하는 메모리가 힙 메모리이다.

### 4) 생성자 (constructor)

`constructor` 가 하는 일은 클래스를 처음 만들 때 멤버변수나 상수를 초기화하는 것이다.

```java
package constructor;
public class Person {
  String name;
  float height;
  float weight;

  public Person(String name) { // 사람 이름을 매개변수로 입력 받아서 Person 클래스를 생성하는 생성자
    this.name = name; 
  }
  public Person () {}   // 자바 컴파일러가 자동으로 만들어 주는 디폴트 생성자.
}                       // 매개변수를 입력받는 생성자가 있을경우, 디폴트 생성자가 없다면 Person()에 오류가 발생한다.
  public Person(String name, float height, float weight) {
    this.name = name;
    this.height = height;
    this.weight = weight;
  }
  // 생성자가 두개 이상 제공되는 경우를 생성자 오버로드라고 한다.
  // 원하는 생성자를 선택해 사용 가능하며, 디폴트 생성자가 필요 없는 경우도 있다.
```

```java
package constructor;
public class PersonTest {
  public static void main(String[] args) {
    Person personLee = new Person();                    // Person()이 생성자
    Person personPark = new Person("박종운", 170, 60);  // 매개변수가 여러개인 생성자
  }
}
```

### 5) 참조 자료형

```java
// 기존 학생 클래스
public class Student2 {
  int studentID;
  String studentName;
  int koreaScore;
  int mateScore;
  String koreaSubject;  // 과목이름 변수를 새로운 클래스로 분리
  String koreaSubject;  // 과목이름 변수를 새로운 클래스로 분리
}
```

```java
// 분리한 과목 클래스
public class Subject {
  String subjectName;
  int scorePoint;
}
```

```java
// 새로운 학생 클래스
public class Student3 {
  int studentID;
  String studenName;
  Subject korean; // Subject 형을 사용하여 선언
  Subject math;   // Subject 형을 사용하여 선언
}
// korean.subjectName math.subjectName 으로 사용 가능
```

### 6) 정보 은닉 (접근 제어자)

`public` `private` `protected`

예약어를 사용해 클래스 내부의 변수나 메서드, 생성자에 대한 접근 권한을 지정할 수 있다. 이러한 예약어를 `접근 제어자(access modifier)`라고 한다. `public`은 외부에서 사용 가능하다는 뜻이고, `private`으로 선언하면 외부 클래스에서 사용이 불가하다.

클래스 내부에서 사용할 변수나 메서드에는 `private`으로 선언하여 얘기치 않은 오류 발생을 막을 수 있다.

`protected` : 같은 패키지 내부와 상속 관계의 클래스에서만 접근할 수 있고 그 외 클래스에서는 접근할 수 없다.

`get()` `set()` 메서드를 사용하면 `private` 변수에 직접 접근하지 않고, `public` 메서드를 통해 `private` 변수에 접근할 수 있다.

```java
package hiding;
public class Student {
 int studentID;
 private String studentName;
 int grade;
 String adress;
 
 public String getStudentName() {  // getter
  return studentName;              // private 변수인 studentName에 접근해 값을 가져옴
 }
 public void setStudentName(String studentName) { // setter
  this.studentName = studentName;                 // private 변수인 studentName에 접근해 값을 지정함
 }
}
```

```java
package hiding;
public class StudentTest {

 public static void main(String[] args) {
  Student studentPark = new Student();
  studentPark.setStudentName("박종운");
  
  System.out.println(studentPark.getStudentName());
 }
}
```

- `getter setter`는 이클립스에서 자동으로 생성 가능하다.  
멤버변수가 속해 있는 클래스 내부에서 `우클릭 > Source > Generate Getters and Setters`

## 교제 코딩연습

```java
// 사칙 연산 함수 완성하기 p.138
package classpart;
public class FunctionTest {
 public static void main(String[] args) {
  int num1 = 10;
  int num2 = 20;
  
  int sum = add(num1, num2);
  System.out.println(num1 + " + " + num2 + " = " + sum + " 입니다.");
  
  int minus = minus(num1, num2);
  System.out.println(num1 + " - " + num2 + " = " + minus + " 입니다.");
  
  int multiple = multiple(num1, num2);
  System.out.println(num1 + " X " + num2 + " = " + multiple + " 입니다.");
  
  double divide = divide(num1, num2);
  System.out.println(num1 + " / " + num2 + " = " + divide + " 입니다.");
 }
 public static int add(int n1, int n2) {
  int result = n1 + n2;
  return result;
 }
 public static int minus(int n1, int n2) {
  int result = n1 - n2;
  return result;
 }
 public static int multiple(int n1, int n2) {
  int result = n1 * n2;
  return result;
 }
 public static double divide(double n1, double n2) {
  double result = n1 / n2;
  return result;
 }
}
```

```java
// 나혼자코딩 p.152 1-1
package Example;
public class Person {
 public int age;
 public String name;
 public boolean isMarried;
 public int child;
}
```

```java
// 나혼자코딩 p.152 1-2
package Example;
public class PersonTest{
 public static void main(String[] args) {
  Person person = new Person();
  person.age = 40;
  person.name = "James";
  person.isMarried = true;
  person.child = 3;
  
  System.out.println(person.age);
  System.out.println(person.name);
  System.out.println(person.isMarried);
  System.out.println(person.child);
 }
}
```

```java
// 나혼자코딩 p.152 2-1
package Example;
public class Order {
 String number;
 String iD;
 String date;
 String customerName;
 String sku;
 String address;
}
```

```java
// 나혼자코딩 p.152 2-2
package Example;
public class OrderTest {
 public static void main(String[] args) {
  Order order1 = new Order();
  order1.number = "201803120001";
  order1.iD = "abc123";
  order1.date = "2018년 3월 12일";
  order1.customerName = "홍길순";
  order1.sku = "PD0345-12";
  order1.address = "서울시 영등포구 여의도동 20번지";
  
  System.out.println("주문 번호 : " + order1.number);
  System.out.println("주문자 아이디 : " + order1.iD);
  System.out.println("주문 날짜 : " + order1.date);
  System.out.println("주문자 이름 : " + order1.customerName);
  System.out.println("주문 상품 번호 : " + order1.sku);
  System.out.println("배송 주소 : " + order1.address);
 }
}
```

## 연습문제

1. 클래스를 생성할 때 호출하는 `생성자`는 멤버 변수를 초기화 하는 데 사용합니다.
2. 클래스를 생성하여 메모리에 있는 상태를 `인스턴스`라 하고 멤버 변수를 다른 말로 `인스턴스 변수`라고 합니다.
3. `메서드`는 일반 함수에 객체 지향의 개념을 추가하여, 클래스 내부에 선언하고 클래스 멤버 변수를 사용하여 클래스 기능을 구현합니다.
4. 05-7 예제 MyDate MyDateTest 완성하기
