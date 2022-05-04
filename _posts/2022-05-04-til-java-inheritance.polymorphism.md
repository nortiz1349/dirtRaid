---
title: "JAVA 상속과 다형성"
excerpt: inheritance, polymorphism
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - inheritance
  - polymorphism

toc: true
toc_sticky: true
date: 2022-05-04
---

## 상속 (inheritance)

객체 지향 프로그래밍의 중요한 특징 중 하나이다. 상위 클래스의 멤버변수와 메서드를 하위클래스가 물려 받아 사용할 수 있게 한다. 프로그램을 수정하거나 새로운 것을 추가 하는데 엄청난 유연성을 부여하게 된다.

- 상위클래스 : `super class` `base class` `parent class`
- 하위클래스 : `sub class` `derived class` `child class`
- `하위클래스` -▷ `상위클래스` (화살표 방향 유의)

```java
// 클래스 상속 문법
class B extends A {
  ...
}
// 클래스 B가  A 클래스를 상속 받는다.
```

A가 가지고 있는 속성이나 기능을 추가로 확장하여 B 클래스를 구현한다는 뜻이다.

상속 관계에서는 상위 클래스가 하위 클래스보다 일반적인 개념이고, 하위 클래스는 상위 클래스보다 구체적인 클래스가 된다.

## 상속에서 클래스 생성과 형 변환

### 하위 클래스가 생성되는 과정

상속을 받은 하위 클래스는 상위 클래스의 변수와 메서드를 사용할 수 있다. 변수를 사용할 수 있다는 것은 그 변수를 저장하고 있는 메모리가 존재한다는 뜻이다. 상위 클래스를 상속 받은 하위 클래스가 생성될 때는 반드시 상위 클래스의 생성자가 먼저 호출 된다.

상위 클래스 생성자가 호출될 때 상위 클래스의 멤버 변수가 메모리에 먼저 생성되고 그 다음 하위 클래스 생성자 호출, 멤버 변수 메모리에 생성된다.

### 부모를 부르는 예약어 `super()`

이 과정에서 하위 클래스 생성자만 호출 했는데도 상위 클래스 생성자를 호출하는 이유는 하위클래스 생성자에서 `super()`를 자동으로 호출하기 때문이다. `super()`는 하위 클래스에서 상위 클래스로 접근할 때 사용하는데, 바이트 코드로 변환되기 전에 컴파일러가 자동으로 코드를 추가한다.

`super()`를 호출하면 상위 클래스의 디폴트 생성자가 묵시적으로 호출되며, 상위 클래스에 디폴트 생성자가 없거나 매개변수가 있는 생성자를 호출하기 위해서는 `super()`에 매개변수를 추가하여 명시적으로 상위 클래스 생성자를 직접 호출해야 한다.

- 상위 클래스의 멤버 변수나 메서드를 참조할 때도 `super`를 사용한다. ex) `super.메서드()`

### 상위 클래스로 묵시적 클래스 형 변환

하위클래스로 인스턴스를 생성할 때 이 인스턴스의 자료형을 상위클래스 형으로 `클래스 형 변환`하여 선언할 수 있다.

```java
// 상위클래스 참조변수명 = new 하위클래스(); 
Customer vc = new VIPCustomer();
```

위 명령이 실행되면 `VIPCustomer` 생성자가 호출되고 클래스 변수가 메모리에 만들어진다. 클래스가 형 변환 되었을 때는 선언한 클래스형에 기반하여 멤버 변수와 메서드에 접근할 수 있다. 따라서 `vc` 참조변수가 가리킬 수 있는 메서드는 `Customer` 클래스의 멤버뿐이다.

- 클래스의 상속 계층 구조가 여러 단계일 경우에도 묵시적으로 형 변환이 된다.

## 메서드 오버라이딩 Method Overriding

### 상위 클래스 메서드 재정의 하기

상위 클래스에 정의한 메서드가 하위클래스에서 구현할 내용과 맞지 않을 경우 하위 클래스에서 이 메서드를 재정의 할 수 있다. 이를 `메서드 오버라이딩(method overriding)`이라고 한다.

오버라이딩 하려면 반환형, 메서드 이름, 매개변수 개수, 매개변수 자료형이 반드시 같아야 한다. 그렇지 않으면 컴파일러는 재정의한 매서드를 다른 메서드로 인식한다.

이클립스의 `Source > Override/Implement Methods...` 기능을 활용하여 자동으로 재정의 할 메서드를 생성할 수 있다. 이 때 `@Override` 애노테이션은 '이 메서드는 재정의된 메서드입니다.'라고 컴파일러에게 명확히 알려 주는 역할을 한다.

- 표준 애노테이션  

  - `@Override` : 재정의된 메서드라는 정보 제공  
  - `@FunctionalInterface` : 함수형 인터페이스라는 정보 제공  
  - `@Deprecated` : 이후 버전에서 사용되지 않을 수 있는 변수, 메서드에 사용  
  - `@SuppressWarnings` : 특정 경고가 나타나지 않도록 함

### 묵시적 클래스 형 변환과 메서드 재정의

상속에서 상위클래스와 하위클래스에 같은 이름의 메서드가 존재할 때 호출되는 메서드는 인스턴스에 따라 결정된다. 다시말해 선언된 클래스형이 아닌 생성된 인스턴스의 메서드를 호출하는 것이다. 이렇게 인스턴스의 메서드가 호출되는 기술을 `가상 메서드(virtual method)`라고 한다.

### 가상 메서드

메서드의 명령 집합은 메서드 영역(코드 영역)에 위치한다. 우리가 메서드를 호출하면 메서드 영역의 주소를 참조하여 명령이 실행되고, 인스턴스가 달라도 동일한 메서드가 호출된다.

가상메서드의 경우에는 `가상 메서드 테이블`이 만들어지고 각 메서드 이름과 실제 메모리 주소가 짝을 이루고 있다. 어떤 메서드가 호출되면 이 테이블에서 주소 값을 찾아서 해당 메서드의 명령을 수행한다.

결과적으로 상속관계에서 `재정의`된 메서드는 두 클래스에서 서로 다른 메서드 주소를 가지고 있고, 실제 인스턴스에 해당하는 메서드가 호출되게 된다. 재정의 되지 않은 메서드는 메서드 주소가 같으며 상위 클래스의 메서드가 호출된다.

자바의 모든 메서드는 가상 메서드이다.

## 다형성 (polymorphism)

다형성이란 하나의 코드가 여러 자료형으로 구현되어 실행되는 것을 말한다. 추상 클래스, 인터페이스에서 구현되며 유연성, 재활용성, 유지보수성에 기본이 되는 객체 지향의 중요한 특성 중 하나이다.

하나의 클래스를 상속 받은 여러 클래스가 있는 경우, 각 클래스마다 같은 이름의 서로 다른 메서드를 재정의 했을 때, 상위 클래스 타입으로 선언된 하나의 변수가 여러 인스턴스에 대입되어 다양한 구현이 실행 될 수 있다.

```java
// 다형성 테스트
package polymorphism;
class Animal {
 public void move() {
  System.out.println("animal move.");
 }
}
class Human extends Animal {
 public void move() {
  System.out.println("Human walk.");
 }
}
class Tiger extends Animal {
 public void move() {
  System.out.println("Tiger Run.");
 }
}
class Eagle extends Animal {
 public void move() {
  System.out.println("Eagle Fly.");
 }
}

public class AnimalTest1 {
 public static void main(String[] args) {
  AnimalTest1 aTest = new AnimalTest1();
  aTest.moveAnimal(new Human());
  aTest.moveAnimal(new Tiger());
  aTest.moveAnimal(new Eagle());
 }
 
 public void moveAnimal(Animal animal) {  // 매개변수의 자료형이 상위 클래스
  animal.move();                          // 재정의된 메서드가 호출됨
 }
}
// Human walk.
// Tiger Run.
// Eagle Fly.
```

Animal에서 상속받은 클래스가 매개변수로 넘어오면서 Animal 형으로 형변환 되므로 `animal.move()` 메서드를 호출할 수 있다.

`animal.move()` 가 호출하는 메서드는 `Animal` 클래스의 `move()`가 아닌 매개변수로 넘어온 실제 인스턴스의 메서드이다.

따라서 `animal.move()` 코드는 변함이 없지만 어떤 매개변수가 넘어왔느냐에 따라 출력문이 달라진다.

## 다형성 활용

- 고객 관리 프로그램

```text
일반고객 : 포인트 적립 1%
VIP : 포인트 5% 적립, 10% 할인, 전담 상담원
```

```java
package polymorphism;

public class Customer {
 protected int customerID;
 protected String customerName;
 protected String customerGrade;
 int bonusPoint;
 double bonusRatio;
 
 public Customer() {
  initCustomer();
 }
 
 public Customer(int customerID, String customerName) {
  this.customerID = customerID;
  this.customerName = customerName;
  initCustomer();
 }
 
 private void initCustomer() {
  customerGrade = "Silver";
  bonusRatio = 0.01;
 }
 
 public int calcPrice (int price) {
  bonusPoint += price * bonusRatio;
  return price;
 }
 
 public int getCustomerID() {
  return customerID;
 }

 public String getCustomerName() {
  return customerName;
 }

 public void setCustomerID(int customerID) {
  this.customerID = customerID;
 }

 public void setCustomerName(String customerName) {
  this.customerName = customerName;
 }

 public String showCusmomerInfo() {
  return customerName + "'s Grade is " + customerGrade + ", Bonus point is " + bonusPoint;
 }
}
```

```java
package polymorphism;

public class VIPCustomer extends Customer {
 private int agentID;
 double saleRatio;
 
 public VIPCustomer(int customerID, String customerName, int agentID) {
  super(customerID, customerName);
  customerGrade = "VIP";
  bonusRatio = 0.05;
  saleRatio = 0.1;
  this.agentID = agentID;
 }

 @Override
 public int calcPrice(int price) {
  bonusPoint += price * bonusRatio;
  return price - (int)(price * saleRatio);
 }

 @Override
 public String showCusmomerInfo() {
  return super.showCusmomerInfo() + " Your agent number is " + agentID;
 }
 
 public int getAgentID () {
  return agentID;
 }

}
```

```java
package polymorphism;

public class CustomerTest {

 public static void main(String[] args) {
  Customer customerLee = new Customer();
  customerLee.setCustomerID(2202);
  customerLee.setCustomerName("Lee");
  customerLee.bonusPoint = 1000;
  
  System.out.println(customerLee.showCusmomerInfo());
  
  Customer customerKim = new VIPCustomer(3001, "Kim", 10);
  customerKim.bonusPoint = 5000;
  
  System.out.println(customerKim.showCusmomerInfo());
  
  int price = 10000;
  int leePrice = customerLee.calcPrice(price);
  int kimPrice = customerKim.calcPrice(price);
  
  System.out.println(customerLee.getCustomerName() + " pay " + leePrice);
  System.out.println(customerLee.showCusmomerInfo());

  System.out.println(customerKim.getCustomerName() + " pay " + kimPrice);
  System.out.println(customerKim.showCusmomerInfo());
 }
}

/* Lee's Grade is Silver, Bonus point is 1000
 * Kim's Grade is VIP, Bonus point is 5000 Your agent number is 10
 * Lee pay 10000
 * Lee's Grade is Silver, Bonus point is 1100
 * Kim pay 9000
 * Kim's Grade is VIP, Bonus point is 5500 Your agent number is 10 
 */
```

```text
고객 등급추가
Gold : 포인트 2% 적립, 5% 할인
```

```java
package polymorphism;

public class GoldCustomer extends Customer {
 double saleRatio;
 
 public GoldCustomer(int customerID, String customerName) {
  super(customerID, customerName);
  customerGrade = "Gold";
  bonusRatio = 0.02;
  saleRatio = 0.05;
 }

 @Override
 public int calcPrice(int price) {
  bonusPoint += price * bonusRatio;
  return price - (int)(price * saleRatio);
 }
}
```

```text
배열로 고객 5명 구현하기
VIP 1명, Gold 2명, Silver 2명
각각 10000원 상품 구매했을 때 결과 출력
```

```java
package polymorphism;
import java.util.ArrayList;
public class CustomerTest {

 public static void main(String[] args) {
  ArrayList<Customer> customerList = new ArrayList<Customer>(); // Customer 형으로 객체 배열 ArrayList 선언
  
  Customer customerLee = new Customer(1001, "Lee");
  Customer customerKim = new Customer(1002, "Kim");
  Customer customerPark = new GoldCustomer(2001, "Park");
  Customer customerSon = new GoldCustomer(2002, "Son");
  Customer customerKang = new VIPCustomer(3001, "Kang", 9001);
  
  customerList.add(customerLee);
  customerList.add(customerKim);
  customerList.add(customerPark);
  customerList.add(customerSon);
  customerList.add(customerKang);
  
  for(Customer customer : customerList) {
   System.out.println(customer.showCusmomerInfo());
  }
  
  int price = 10000;
  for (Customer customer : customerList) { // 향상된 FOR 문, 다형성 구현
   int cost = customer.calcPrice(price);
   System.out.println(customer.getCustomerName() + " pay " + cost);
   System.out.println(customer.getCustomerName() + "'s BonusPoint is " + customer.bonusPoint);
  }
 }
}

/* Lee's Grade is Silver, Bonus point is 0
 * Kim's Grade is Silver, Bonus point is 0
 * Park's Grade is Gold, Bonus point is 0
 * Son's Grade is Gold, Bonus point is 0
 * Kang's Grade is VIP, Bonus point is 0 Agent number is 9001
 * Lee pay 10000
 * Lee's BonusPoint is 100
 * Kim pay 10000
 * Kim's BonusPoint is 100
 * Park pay 9500
 * Park's BonusPoint is 200
 * Son pay 9500
 * Son's BonusPoint is 200
 * Kang pay 9000
 * Kang's BonusPoint is 500
 */
```

### 상속을 항상 사용하는 것이 좋을까?

물론 아니다. 상속은 IS-A 관계(is a relationship; inheritance)에서 사용하는 것이 가장 효율적이다. ex) "사람은 포유류이다"

일반 클래스를 점차 구체화하는 상황에서 상속을 사용하는 것이다. 하위 클래스는 상위 클래스에 종속되기 때문에 이질적인 클래스 간에는 상속을 사용하지 않는 것이 좋다.

이와 반대로 HAS-A 관계(has a relationship; association)도 있다. HAS-A 관계는 한 클래스가 다른 클래스를 소유하는 관계로써 상속을 사용하지 않는게 좋다. 일반적으로 HAS-A 관계라면 멤버 변수로 사용하는 것이 적절하다.

## 다운 캐스팅과 instanceof

아래 코드로 생성된 인스턴스 Human은 Animal형으로 업캐스팅(up casting) 되었다.

```java
Animal ani = new Human();
```

 이렇게 되면 Human 클래스에서 더 많은 메서드와 멤버 변수가 있다 하더라도 사용 할 수 없고, Animal 클래스에서 선언한 메서드와 멤버 변수만 사용 할 수 있다.

 따라서 필요에 따라 원래 인스턴스의 자료형(Human형)으로 되돌아 가야하는 경우가 있다. 이 때 원래 자료형으로 형 변환하는 것을 다운 캐스팅(down casting)이라고 한다.

### instanceof

 다운캐스팅을 하기 전에 상위 클래스로 형 변환된 인스턴스의 원래 자료형을 확인해야 반환할 때 오류를 막을 수 있다. 이를 확인 하는 예약어가 `instanceof` 이다.

 ```java
Animal foo = new Human();
if(foo instanceof Human) {  // foo 인스턴스 자료형이 Human형이라면 (true)
  Human human = (Human)foo; // 인스턴스 foo를 Human형으로 다운캐스팅
}
 ```

상위 클래스로는 묵시적으로 형 변환이 되지만, 하위 클래스로 형변환을 할때는 `Human human = (Human)foo;` 문장과 같이 명시적으로 변환해야 한다.
