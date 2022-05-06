---
title: "JAVA 인터페이스"
excerpt: interface
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - interface

toc: true
toc_sticky: true
date: 2022-05-06
---

## 인터페이스란?

인터페이스는 추상 메서드와 상수로만 이루어져 있고, 클래스 혹은 프로그램이 제공하는 기능을 명시적으로 선언하는 역할을 한다. 구현된 코드가 없기 때문에 당연히 인스턴스를 생성할 수도 없다.

### 인터페이스 만들기

- interface

  ```java
  package interfaceex;

  public interface Calc {
  /* 인터페이스에서 선언한 변수는 컴파일 과정에서 상수로 변환됨. */
  double PI = 3.14;
  int ERROR = -999999999;
  
  /* 인터페이스에서 선언한 메서드는 컴파일 과정에서 추상 메서드로 변환됨. */
  int add(int num1, int num2);
  int substract(int num1, int num2);
  int times(int num1, int num2);
  int divide(int num1, int num2);
  }
  ```

### 클래스에서 인터페이스 구현하기

- implement : 인터페이스에 선언한 기능을 클래스가 구현한다는 의미로 `implements` 예약어를 사용한다.

  ```java
  package interfaceex;

  public class Calculator implements Calc{ // Calculator 오류 발생
  }
  ```

  `add unimplemented methods` 옵션으로 `add()` `substract()` 추상 메서드 2개만 구현한다. 모든 추상 메서드가 구현되지 않았으므로 추상 클래스로 만든다.

  ```java
  package interfaceex;
  
  public abstract class Calculator implements Calc{
    
    @Override
    public int add(int num1, int num2) {
      return num1 + num2;
    }
    
    @Override
    public int substract(int num1, int num2) {
      return num1 - num2;
    }
  }
  ```

### 클래스 완성하고 실행하기

```java
package interfaceex;

public class CompleteCalc extends Calculator {
 
 /* 구현되지 않았던 나머지 추상메서드를 구현하였다. */
 @Override
 public int times(int num1, int num2) {
  return num1 * num2;
 }

 @Override
 public int divide(int num1, int num2) {
  if (num2 != 0)
   return num1 / num2;
  else
   return Calc.ERROR;
 }

 public void showInfo() {
  System.out.println("Calc interface implemented.");
 }
}
```

```java
package interfaceex;

public class CalculatorTest {

 public static void main(String[] args) {
  int num1 = 10;
  int num2 = 5;
  
  /* Calc, Calculator 클래스는 인스턴스 생성이 불가하다. */
  CompleteCalc calc = new CompleteCalc();
  System.out.println(calc.add(num1, num2));
  System.out.println(calc.substract(num1, num2));
  System.out.println(calc.times(num1, num2));
  System.out.println(calc.divide(num1, num2));
  calc.showInfo();
 }
}
```

### 인터페이스 구현과 형 변환

`CompleteCalc` 클래스는 `Calc` 인터페이스를 구현하였으므로 `Calc` 형으로 형변환 할 수 있다.

```java
...
  Calc calc = new CompleteCalc();
  System.out.println(calc.add(num1, num2));
  System.out.println(calc.substract(num1, num2));
  System.out.println(calc.times(num1, num2));
  System.out.println(calc.divide(num1, num2));
  calc.showInfo();  // 에러 발생, Calculator 메서드임.
...
```

`Calc`형으로 선언한 변수에서 사용할 수 있는 메서드는 `Calc`에서 선언한 메서드뿐이다. 그러므로 `Calculator`에서 선언한 `showInfo()` 메서드는 사용할 수 없게 된다.

## 인터페이스와 다형성

인터페이스는 클라이언트 프로그램에 어떤 메서드를 제공 하는지 미리 알려주는 명세(specification) 또는 약속의 역할을 한다. 즉 클래스의 구현 코드 전체를 살펴보지 않고 인터페이스 선언부만 봐도 클래스를 어떻게 사용할지 알 수 있다.

인터페이스를 사용하면 다형성을 구현하여 확정성 있는 프로그램을 만들 수 있다. 사용할 인스턴스가 어떤 클래스로 생성되었는지와 상관 없이 인터페이스에서 제공하는 메서드를 호출하여 다형성을 구현한다. 그리고 클라이언트 프로그램을 많이 수정하지 않고 기능을 추가하거나 다른 기능을 사용할 수 있다.

## 인터페이스 요소 살펴보기

### 인터페이스 상수

인터페이스에 선언한 변수를 컴파일하면 상수로 변환된다.

```java
...
double PI = 3.14;
// ==
public static final double PI = 3.14;
...
```

### 디폴트 메서드와 정적 메서드

- 디폴트 메서드(default method) : 인터페이스에서 구현 코드까지 작성한 메서드
  
  ```java
  ...
  default void description() {  // 디폴트 메서드 구현
    System.out.println("Integer Calculator implemented.");
  }
  ...
  ```

  ```java
  ...
  ComleteCalc calc = new CompleteCalc(); // 클래스 생성
  ...
  calc.description(); // 디폴트 메서드 호출
  ...
  ```

  말 그대로 기본으로 제공되는 메서드. 인터페이스에서 구현하지만, 이후 인터페이스를 구현한 클래스가 생성되면 그 클래스에서 사용할 기본 기능이다.
  
  `default` 예약어를 사용하여 구현하고, 디폴트 메서드를 사용하려면 클래스를 먼저 생성해야 한다. 만약 디폴트 메서드가 새로 생성한 클래스에서 원하는 기능과 맞지 않다면 하위 클래스에서 재정의할 수 있다.
  
- 정적 메서드 (static method) : 인스턴스 생성과 상관없이 사용할 수 있는 메서드
  
  `static` 예약어를 사용하여 선언하며 클래스 생성과 무관하게 사용할 수 있다. 정적 메서드를 사용할 때는 인터페이스 이름으로 직접 참조하여 사용한다.

  ```java
  ...
  static int total(int[] arr) { // 인터페이스에 정적 메서드 total() 구현
    ...
  }
  ```

  ```java
  ...
  System.out.println(Calc.total(arr));  // 정적 메서드 사용하기
  ...
  ```

### private 메서드

`private` 메서드는 인터페이스를 구현한 클래스에서 사용하거나 재정의할 수 없다. 따라서 기존에 구현된 코드를 변경하지 않고 공통으로 사용하는 경우에 `private` 메서드로 구현하면 코드 재사용성을 높일 수 있다.

`private` 메서드는 코드를 모두 구현해야 하므로 추상 메서드에 사용할 수는 없지만 `static` 예약어는 함께 사용할 수 있다.

## 인터페이스 활용하기

### 한 클래스가 여러 인터페이스를 구현하는 경우

```java
...
public class [클래스이름] implements [인터페이스1], [인터페이스2] {
  ...
}
```

한 클래스가 여러 클래스를 상속 받으면 메서드 호출이 모호해지는 문제가 발생할 수 있다. 하지만 인터페이스는 구현 코드나 멤버 변수를 가지지 않기 때문에 한 클래스가 여러 인터페이스를 구현할 수 있다. 두 인터페이스에 같은 메서드가 선언되었다고 해도 구현은 클래스에서 이루어지므로, 어떤 메서드를 호출해야 하는지 모호하지 않은 것이다.

정적 메서드의 경우 인스턴스 생성과 상관없이 사용할 수 있고, 두 인터페이스에 동일한 이름의 메서드가 있다 하더라도 인터페이스 이름을 특정하여 호출하므로 사용하는데 문제가 되지 않는다.

디폴트 메서드는 인스턴스를 생성해야 호출할 수 있는 메서드이기 때문에, 같은 이름의 디폴트 메서드가 두 인터페이스에 있으면 문제가 된다. 같은 이름의 메소드가 있는 인터페이스 둘을 구현하면 디폴트 메서드가 중복되었다는 오류가 발생하며, 디폴트 메서드를 재정의해야 사용할 수 있다.

### 인터페이스 상속하기

```java
...
public interface [인터페이스이름] extends [상위인터페이스1],[상위인터페이스2] {
  ...
}
```

인터페이스 간 상속은 구현 코드를 통해 기능을 상속하는 것이 아니므로 형 상속(type inheritance)이라고 부른다. 상속받은 인터페이스는 상위 인터페이스에 선언한 추상 메서드를 모두 가지게 된다.

생성한 클래스는 상위 인터페이스형으로 형변환 할 수 있고 상위 인터페이스에 선언한 메서드만 호출할 수 있다.

인터페이스 상속은 인터페이스를 정의할 때 기능상 계층 구조가 필요한 경우에 사용한다.

### 인터페이스 구현과 클래스 상속 함께 쓰기

한 클래스에서 클래스 상속과 인터페이스 구현을 모두 할 수 있다.

![image](https://user-images.githubusercontent.com/86943021/167063227-6119a34f-4463-4883-968c-5e270f050a60.png)

```java
/* Shelf 클래스 만들기 */
package bookshelf;
import java.util.ArrayList;

public class Shelf {
 /* 자료를 순서대로 저장할 ArrayList 선언 */
 protected ArrayList<String> shelf;
 
 /* 디폴트 생성자로 Shelf 클래스를 생성하면 ArrayList도 생성 */
 public Shelf () {
  shelf = new ArrayList<String>();
 }
 
 public ArrayList<String> getShelf() {
  return shelf;
 }
 
 public int getCount() {
  return shelf.size(); // 배열 shelf에 저장된 요소 수를 반환함.
 }
}
```

```java
/* Queue 인터페이스 정의 */
package bookshelf;

public interface Queue {
 void enQueue(String title); // 배열의 맨 마지막에 추
 String deQueue();           // 배열의 맨 처음 한목 반
 int getSize();              // 현재 Queue에 있는 개수 반
}
```

`Shelf` 클래스와 `Queue` 인터페이스를 구현하는 `BookShelf` 클래스 만들기

```java
package bookshelf;

public class BookShelf extends Shelf implements Queue {

 @Override
 public void enQueue(String title) {
  shelf.add(title);
 } // 배열에 요소 추가

 @Override
 public String deQueue() {
  return shelf.remove(0);
 } // 맨 처음 요소를 배열에서 삭제하고 반환

 @Override
 public int getSize() {
  return getCount();
 } //현재 Queue에 있는 개수 반환
}
```

```java
package bookshelf;

public class BookShelfTest {

 public static void main(String[] args) {
  Queue shelfQueue = new BookShelf();
  shelfQueue.enQueue("book 1");
  shelfQueue.enQueue("book 2");
  shelfQueue.enQueue("book 3");
  
  System.out.println(shelfQueue.deQueue());
  System.out.println(shelfQueue.deQueue());
  System.out.println(shelfQueue.deQueue());
 }
}

/* book 1
 * book 2
 * book 3
 */
```

- 고객 상담 전화 배분 프로그램

![image](https://user-images.githubusercontent.com/86943021/167084209-667b67e8-7f62-41de-82b0-998224c4f64e.png)

```java
package scheduler;

public interface Schedular {
 public void getNextCall();
 public void sendCallToAgent();
}
```

```java
package scheduler;

public class RoundRobin implements Schedular{

 @Override
 public void getNextCall() {
  System.out.println("Get next call.");
 }

 @Override
 public void sendCallToAgent() {
  System.out.println("Send call to next agent.");
 }
}
```

```java
package scheduler;

public class LeastJob implements Schedular{

 @Override
 public void getNextCall() {
  System.out.println("Get next call.");
 }

 @Override
 public void sendCallToAgent() {
  System.out.println("Send call to Least job agent.");
 }
}
```

```java
package scheduler;

public class PriorityAllocation implements Schedular{

 @Override
 public void getNextCall() {
  System.out.println("Get Next call - High priority customer.");
 }

 @Override
 public void sendCallToAgent() {
  System.out.println("Send call to High skilled Agent first.");
 }
}
```

```java
package scheduler;
import java.io.IOException;

public class SchedularTest {

 public static void main(String[] args) throws IOException {
  System.out.println("choose one");
  System.out.println("R: Round Robin");
  System.out.println("L: Least Job");
  System.out.println("P: High Priority customer");
  
  int ch = System.in.read();    // 할당 방식을 입력 받아 ch 변수에 대입
  Schedular schedular = null;
  
  if (ch == 'R' || ch == 'r') { 
   schedular = new RoundRobin();
  } 
  else if (ch == 'L' || ch == 'l') {
   schedular = new LeastJob();
  }
  else if (ch == 'P' || ch == 'p') {
   schedular = new PriorityAllocation();
  }
  else {
   System.out.println("wrong key!");
   return;
  }
  /* 어떤 정책인가와 상관 없이 인터페이스에 선언한 메서드 호출 */
  schedular.getNextCall();
  schedular.sendCallToAgent();
 }
}
```
