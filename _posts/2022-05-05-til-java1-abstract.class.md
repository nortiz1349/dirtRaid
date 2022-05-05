---
title: "JAVA 추상 클래스"
excerpt: abstract class, template method, final
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - abstract
  - final
  - template method

toc: true
toc_sticky: true
date: 2022-05-05
---

## 추상 클래스 abstract

추상 클래스는 항상 추상 메서드를 포함한다.

```java
// 일반적인 메서드
int add(int x, int y) {
  return x + y;
}
```

일반적인 메서드는 `{ }` 로 감싼 구현부(implementaion)가 있지만 아래 추상 메서드는 구현 코드가 없다.(body가 없다.)

```java
// 추상 메서드
abstract int add(int x, int y);
```

`abstract` 예약어로 메서드를 '선언만' 했다.

### 메서드 선언의 의미

프로그램을 실제로 구현하기 전에 먼저 어떻게 구현할지 결정하는 과정이 필요하다. 이런 과정을 개발 설계라고 한다.

```java
int add(int num1, int num2)
```

위  코드처럼 선언한 메서드를 보면 두 개의 정수를 입력 받은 후 더해서 그 결과 값을 반환한다는 것을 유추할 수 있다. 즉 이 메서드의 선언부(declaration)만 봐도 어떤 일을 하는 메서드인지 알 수 있는 것이다.

함수의 선언부 즉 반환 값, 함수 이름, 매개변수를 정의한다는 것은 곧 함수의 역할이 무엇인지, 어떻게 구현해야 하는지를 정의한다는 뜻이다. 따라서 함수 몸체를 구현하는 것보다 중요한 것은 함수 선언부를 작성하는 것이다. 자바에서 사용하는 메서드도 마찬가지이다. 메서드를 선언한다는 것은 메서드가 해야 할 일을 명시해 두는 것이다.

### 추상 클래스 구현하기

- 클래스 다이어그램

![image](https://user-images.githubusercontent.com/86943021/166929932-ce19b025-c960-41bf-b159-4ac6ffb51ce3.png)

추상 클래스와 추상 메서드는 기울임꼴로 표현한다.

```java
/* 추상 클래스 구현하기 */
package abstractx;

public abstract class Computer {
 public abstract void display();
 public abstract void typing();
 public void turnOn() {
  System.out.println("Turn on.");
 }
 public void turnOff() {
  System.out.println("Turn off.");
 }
}

```

Computer를 상속 받는 클래스 중 `turnOn()`, `turnOff()` 구현코드는 공통 `display()`, `typing()`은 하위 클래스에 따라 구현이 달라질 수 있다.

```java
/* 추상 클래스 상속 받기 */
package abstractx;

public class DeskTop extends Computer {

 @Override
 public void display() {
  System.out.println("DeskTop display()");
 }

 @Override
 public void typing() {
  System.out.println("DeskTop typing()");

 }
}
// Computer 클래스에 포함된 추상 메서드를 재정의 하였다.
```

추상 클래스를 상속 받은 클래스는 추상 클래스가 가진 메서드를 상속 받는다. 따라서 상속 받은 클래스는 추상 메서드를 포함한다. 추상 클래스를 상속받은 하위 클래스는 구현되지 않은 추상 메서드를 모두 구현해야 구체적인 클래스가 된다.

```java
/* NoteBook 클래스 구현 */
package abstractx;
public abstract class NoteBook extends Computer {

 @Override
 public void display() {
  System.out.println("NoteBook display()");
 }
}
```

이 클래스에서는 상속받은 추상 메서드를 모두 구현하지 않고 `display()` 하나만 구현하였다. 그러므로 상속 받은 추상 메서드가 하나 가지고 있기 때문에 추상 클래스가 된다.

```java
/* MyNoteBook 클래스 구현 */
package abstractx;
public class MyNoteBook extends NoteBook {

 @Override
 public void typing() {
  System.out.println("MyNoteBook typing()");
 }
}
```

모든 추상 메서드가 구현되었고 abstract를 사용하지 않는다.

```java
/* 추상 클래스 테스트 */
package abstractx;
public class ComputerTest {

 public static void main(String[] args) {
  Computer c1 = new Computer(); // 오류: 클래스를 인스턴스로 생성 할 수 없음
  Computer c2 = new DeskTop(); 
  Computer c3 = new NoteBook(); // 오류: 클래스를 인스턴스로 생성 할 수 없음
  Computer c4 = new MyNoteBook();
 }
}
```

추상 클래스는 인스턴스를 생성할 수 없다.

- 문법상으로 모든 메서드를 구현했어도 abstract 예약어를 사용하면 추상 클래스이다. 이 경우에 new 예약어로 인스턴스를 생성할 수 없다.

## 템플릿 메서드

추상 클래스로 구현할 수 있으며 디자인 패턴중 하나이다. 로직의 흐름이나 시나리오를 정의하는 메서드이다.

![image](https://user-images.githubusercontent.com/86943021/166938644-63e4f66b-576c-45a8-9357-6bfb4e1246f6.png)

```java
/* 추상 클래스와 템플릿 메서드 */
package template;
public abstract class Car {
 public abstract void drive();
 public abstract void stop();
 
 public void starCar() {
  System.out.println("Start Car.");
 }
 
 public void turnOff() {
  System.out.println("Turn Off.");
 }
 
 /* 템플릿 메서드 */
 final public void run() {
  starCar();
  drive();
  stop();
  turnOff();
 }
}
```

`run()` 메서드는 자동차가 달리는 방법을 순서대로 구현했다. 만약 `Car` 클래스를 상속 받으면 어떤 자동차든 모두 이 순서대로 동일한 방식으로 달리는 것이다.

템플릿 메서드에서 호출하는 메서드가 추상 메서드라면 차종에 따라 구현 내용이 바뀔 수는 있다. 하지만 시동을 켜고, 달리고, 정지하고, 시동을 끄는 시나리오는 변하지 않는다.

그러므로 템플릿 메서드는 상속 받은 하위 클래스에서 재정의 할 수 없도록 `final` 예약어를 사용해 선언한다.

## final

|사용위치|설명|
|---|---|
|변수|final 변수는 상수를 의미한다.|
|메서드|final 메서드는 하위 클래스에서 재정의할 수 없다.|
|클래스|final 클래스는 상속할 수 없다.|

- `final` 변수

```java
/* 여러 파일에서 공유하는 상수 */
package template;

public class Define {
 public static final int MIN = 1;
 public static final int MAX = 99999;
 public static final int ENG = 1001;
 public static final int MATH = 2001;
 public static final double PI = 3.14;
 public static final String GOOD_MORNING = "Good Morning!";
}
```

상수는 대문자로 표현, IDE에서 자동으로 <i>이탤릭채</i>로 표현된다.
