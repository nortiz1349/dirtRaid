---
title: "JAVA 기본 클래스"
excerpt: Object, String, Wrapper, Class
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - Do it!
  - Class
  - library

toc: true
toc_sticky: true
date: 2022-05-10
---

## :hash: Object 클래스

### :ballot_box_with_check: java.lang

지금까지 공부 해왔던 String, Integer 같은 클래스가 속해 있는 패키지이다. 클래스 전체 이름은 각각 `java.lang.String` `java.lang.Interger` 이다.

자바에서는 외부 패키지의 클래스를 사용하기 위해서는 `import`문을 사용하여 선언 후 사용해야 하지만 `java.lang` 패키지는 컴파일 시에 `import java.lang.*` 문장이 자동으로 추가된다. 가장 많이 사용하는 기본 클래스가 할 수 있겠다.

### :ballot_box_with_check: java.lang.Object

자바 클래스의 최상위 클래스로 모든 클래스는 `Object` 클래스로부터 상속 받는다. 이 또한 마찬가지로 컴파일 과정에 `... extends Object { }` 문장이 자동으로 쓰인다.

모든 클래스가 `Object` 클래스를 상속 받았으므로 `Object`의 메서드를 사용할 수 있고, 재정의할 수도 있고, `Object`형으로 변환 할 수도 있다.

- 주로 사용되는 `Object` 메서드

  |메서드|설명|
  |---|---|
  |`String toString()`|객체를 문자열로 표현하여 반환한다. 재정의하여 객체에 대한 설명이나 특정 멤버 변수 값을 반환한다.|
  |`bloolean equals(Object obj)`|두 인스턴스가 동일한지 여부를 반환한다. 재정의하여 논리적으로 동일한 인스턴스임을 정의할 수 있다.|
  |`int hashCode()`|객체의 해시 코드 값을 반환한다.|
  |`Object clone()`|객체를 복제하여 동일한 멤버 변수 값을 가진 새로운 인스턴스를 생성한다.|
  |`Class getClass()`|객체의 Class 클래스를 반환한다.|
  |`void finalize()`|인스턴스가 힙 메모리에서 제거될 때 가비지 컬렉터(GC)에 의해 호출되는 메서드이다. 네트워크 연결 해제, 열려 있는 파일 스트림 해제 등을 구현한다.|
  |`void wait()`|멀티스레드 프로그램에서 사용하는 메서드이다. 스레드를 '기다리는 상태'(non runnable)로 만든다.|
  |`void notify()`|`wait()` 메서드에 의해 기다리고 있는 스레드를 실행 가능한 상태(runnable)로 가져온다.|
  
  `Object` 메서드는 필요에 따라 재정의하여 사용할 수 있지만 모든 메서드를 재정의 할 수 있는 것은 아니다. `JavaDoc` Method Detail에서 확인 가능한데 `final`로 선언한 메서드는 재정의가 불가하다.

### :ballot_box_with_check: toString()

- `Object` 클래스의 `toString()` 메서드

  ```java
  // 클래스이름@해시코드값 반환
  getClass().getName() + '@ + Interger.toHexString(hashCdoe())
  ```

  인스턴스 정보를 문자열로 반환하는 메서드이다.

- `String`과 `Integer` 클래스의 `toString()` 메서드

  ```java
  String str = new String("test");
  System.out.println(str);    // test 출력됨
  Interger i = new Interger(100);
  System.out.println(i);      // 100 출력됨
  ```

  해당 클래스에서는 `toString()` 메서드가 재정의 되었기 때문에 `클래스이름@해시코드` 값이 아니라 해당 문자열과 정수값이 출력되었다.

- 필요에 따라 직접 재정의하여 사용할 수 있다.

  직접 선언부를 적거나 `[Source -> Override/Implement Methods..]` 기능을 사용할 수 있고, 객체의 참조 변수를 이용해 원하는 문자열을 표현할 수 있다.

### :ballot_box_with_check: equals()

`equals()` 메서드의 원래 기능은 두 인스턴스의 주소 값을 비교하여 boolean 값을 반환해 주는 것이다. 주소 값이 같다면 당연히 같은 인스턴스지만, 서로 다른 주소값을 가질 때도 같은 인스턴스라고 정의할 수 있는 경우가 있다.

물리적 동일성 뿐 아니라 논리적 동일성을 구현할 때 `equals()` 메서드를 재정의 하여 사용한다.

- `Object` 클래스의 `equals()` 메서드

  인스턴스 주소가 달라도 동일한 객체인지를 확인해야할 때가 있다. 두 개의 인스턴스가 있을 때 논리 연산자 `==`는 단순히 물리적으로 같은 메모리 주소인지 여부를 확인할 수 있고, `Object`의 `equals()` 메서드는 재정의하여 논리적으로 같은 인스턴스인지 확인하도록 구현할 수 있다.

  ```java
  ...
  Book book1 = new Book(20, "ants");
  Book book2 = new Book(20, "ants");
  
  System.out.println(book1.equals(book2));
  // 값은 같지만 주소값을 비교하므로 false를 반환한다.
  ```

- `String` `Integer` 클래스의 `equals()` 메서드
  
  문자열 혹은 정수값을 비교하여 같은 값이면 `true`를 반환하도록 재정의 되어 있다.

  ```java
  ...
  String book3 = new String("book3");
  String book4 = new String("book3");
  
  System.out.println(book3.equals(book4));
  // true
  ```

- `equeal()` 메서드를 재정의 했다면  `hashCode()` 메서드도 재정의 해야한다.

### :ballot_box_with_check: hash()

해시(hash)는 정보를 저장하거나 검색할 때 사용하는 자료 구조이다. 정보를 어디에 저장할 것인지, 어디서 가져올 것인지 해시 함수를 사용하여 구현한다.

해시 함수는 객체의 특정 정보(키값)를 매개변수 값으로 넣으면 그 객체가 저장되어야 할 위치나 저장된 해시 테이블 주소(위치)를 반환한다.

자바에서는 인스턴스를 힙 메모리에 생성하여 관리할 때 해시 알고리즘을 사용한다.

```java
hashCode = hash(key);
// 객체의 해시 코드 값(메모리 위치 값)이 반환됨
```

- `String` `Integer` 클래스의 `hashCode()` 메서드

  `equals()` 메서드의 결과 값이 true 인 경우 `hashCode()` 메서드는 동일한 해시 코드 값을 반환한다. `Integer` 클래스는 정수 값을 반환하도록 재정의 되어 있다.

  ```java
  String str1 = new String("Java");
  String str2 = new String("Java");

  System.out.println(str1.hashCode());  // Java 문자열의 해시 코드 값 출력
  System.out.println(str2.hashCode());  // Java 문자열의 해시 코드 값 출력

  Integer i1 = new Integer(100);
  Integer i2 = new Integer(100);
  
  System.out.println(i1.hashCode());  // Integer(100)의 해시 코드 값 출력
  System.out.println(i2.hashCode());  // Integer(100)의 해시 코드 값 출력

  // 2301506
  // 2301506
  // 100
  // 100
  ```

### :ballot_box_with_check: clone()

객체 원본을 유지해 놓고 복사본을 사용한다거나, 기본 틀(prototype)의 복사본을 사용해 동일한 인스턴스를 만들어 복잡한 생성 과장을 간단히 하려는 경우에 `clone()` 메서드를 사용할 수 있다.

```java
package object;

/* 원점을 의미하는 Point 클래스 */
class Point{
 int x;
 int y;
 
 Point(int x, int y){
  this.x = x;
  this.y = y;
 }
 
 public String toString(){
  return "x = " + x + "," + "y = " + y;
 }
}

/* 객체를 복제해도 된다는 의미로 Cloneable 인터페이스 선언 */
class Circle implements Cloneable{
 
 Point point;
 int radius;
 
 Circle(int x, int y, int radius){
  point = new Point(x, y);
 }
 
 public String toString(){
  return "원점은 " + point + "이고," + "반지름은 " + radius + "입니다"; 
 }

 
 @Override
 public Object clone() throws CloneNotSupportedException {
  return super.clone();
 }
}

public class ObjectCloneTest {

 public static void main(String[] args) throws CloneNotSupportedException { // 오류 예외 처리

  Circle circle = new Circle(10, 20, 30);
  Circle copyCircle = (Circle)circle.clone();
  // circle 인스턴스를 copyCircle에 복제

  System.out.println(circle);
  System.out.println(copyCircle);
  // 동일한 값 출력

  System.out.println(System.identityHashCode(circle));
  System.out.println(System.identityHashCode(copyCircle));
  // 개별 주소값 출력
 }
}
```

`clone()` 메서드를 사용하려면 객체를 복제해도 된다는 의미로 `Cloneable` 인터페이스를 구현해야 한다. 구현하지 않으면 `CloneNotSupportedException`이 발생한다.

:bell: `Cloneable` 인터페이스를 선언해도 별도로 구현해야하는 메서드는 없다. 이렇게 구현할 메서드가 없는 인터페이스를 마커 인터페이스(marker interface)라고 한다.

## :hash: String 클래스

C언어에서는 문자열을 char형 배열로 표현하지만, 자바에서는 문자열을 위한 String이라는 클래스를 별도로 제공한다. String 클래스에는 문자열과 관련된 작업을 할 때 유용하게 사용할 수 있는 다양한 메소드가 포함되어 있다.

이러한 String 클래스는 java.lang 패키지에 포함되어 제공된다.

String 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없다. 이러한 객체를 자바에서는 불변 객체(immutable object)라고 한다.

즉, 자바에서 덧셈(+) 연산자를 이용하여 문자열 결합을 수행하면, 기존 문자열의 내용이 변경되는 것이 아니라 내용이 합쳐진 새로운 String 인스턴스가 생성된다.

이는 성능 하락을 의미하며, 문자열의 결합이나 변경이 잦다면 내용을 변경할 수 있는 `StringBuffer`를 사용한다.

## :hash: Wrapper 클래스
