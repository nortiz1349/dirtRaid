---
title: "JAVA 배열과 ArrayList"
excerpt: array, ArrayList
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

## 배열이란?

배열을 사용하면 자료형이 같은 자료 여러개를 한 번에 관리 할 수 있다.

### 1) 배열의 선언

배열을 선언하면 선언한 자료형과 배열 길이에 따라 메모리가 할당 된다.

  ```java
  // 자료형[] 배열이름 = new 자료형[개수]
  int[] studentIDs = new int[10] // int형 요소가 10개인 배열 선언
  ```

### 2) 배열 초기화하기

배열을 선언하는 동시에 각 요소의 값이 초기화 된다. 배열의 자료형에 따라 정수는 `0`, 실수는 `0.0`, 객체 배열은 `null`로 초기화 되는데, 선언과 동시에 특정 값으로 초기화도 가능하다.  

  ```java
  int[] studentIDs = new int[] {101, 102, 103}  // 개수는 생략함.
  int[] studentIDs = new int[3] {101, 102, 103} // 개수를 넣으면 오류 발생
  int[] studentIDs = {101. 102, 103}  // int형 요소가 3개인 배열 생성
  // new int[] 부분은 생략 가능하다.

  int[] studentIDs; // 배열 자료형 선언
  studentIDs = new int[] {101, 102, 103}  // new int[] 생략할 수 없음
  ```

### 3) 배열 사용하기

  ```java
  studentIDs[0] = 10; // 배열의 첫번재 요소에 값 10 지정.
  ```

### 4) 문자 저장 배열 만들기

```java
// abcd 배열에 저장하고 출력하기
package array;
public class CharArray {
 public static void main(String[] args) {
  char[] alphabets = new char[30];  // 자료형 char, 요소 30개 배열 생성
  char ch = 'A';
  
  for(int i=0; i < alphabets.length; i++, ch++) { // 배열의 길이만큼 반복
   alphabets[i] = ch; // 0번 배열부터 ch변수 대입
   System.out.println(alphabets[i] + " " + (int)alphabets[i]);
  }
 }
}
```

### 5) 객체 배열 사용하기

```java
// 객체 배열 사용하기
package array;
public class Book {
 private String bookName;
 private String author;
 
 public Book() {} // 디폴트 생성자
 
 public Book(String bookName, String author) { 
  this.bookName = bookName;
  this.author = author;
 }  // 책 이름과 저자를 매개변수로 하는 생성자
 public String getBookName() {
  return bookName;
 }
 public void setBookName(String bookName) {
  this.bookName = bookName;
 }
 public String getAuthor() {
  return author;
 }
 public void setAuthor(String author) {
  this.author = author;
 }

 public void showBookInfo () {
  System.out.println(bookName + ", " + author);
 }  // 책정보 출력 메서드
}
```

```java
package array;
public class BookArray {
 public static void main(String[] args) {
  Book[] libraryBooks = new Book[5]; 
  // Book 인스턴스가 바로 생성되는 것은 아님.
  // Book의 주소값을 담을 공간을 생성하고 null로 초기화 한 것.
  // 그렇기 때문에 아래 배열을 출력하면 null 반환됨.
  for(int i=0; i < libraryBooks.length; i++) {
   System.out.println(libraryBooks[i]); // null
  }
  // Book 인스턴스 생성 후 배열에 저장.
  libraryBooks[0] = new Book("book1","book1author");
  libraryBooks[1] = new Book("book2","book2author");
  libraryBooks[2] = new Book("book3","book3author");
  libraryBooks[3] = new Book("book4","book4author");
  libraryBooks[4] = new Book("book5","book5author");
  
  for (int i=0; i < libraryBooks.length; i++) {
   libraryBooks[i].showBookInfo();      // 배열에 저장된 책정보 출
   System.out.println(libraryBooks[i]); // 배열의 주소값 출력
  }
 }
}
```

### 6) 배열 복사하기

기존 배열과 자료형 및 배열 크기가 똑같은 배열을 새로 만들때, 혹은 배열의 모든 요소에 자료가 꽉 차서 더 큰 배열을 만들어 기존 배열에 저장된 자료를 가져올 때 사용한다.

`System.arraycopy(src, srcPos, dest, destPos, length)`

|매개변수|설명|
|:---:|:---:|
|src|복사할 배열 이름|
|srcPos|복사할 배열의 첫번째 위치|
|dest|복사해서 붙여 넣을 대상 배열 이름|
|desPos|복사해서 붙여 넣을 첫번째 위치|
|length|src에서 dest로 자료를 복사할 요소 개수|

### 7) 향상된 `for`문과 배열

```java
for(변수:배열) {
  반복 실행문;
}

...
String[] strArray = {"java", "Android", "C", "python"};
for (String lang : strArray) {
  System.out.println(lang);
}

```

## 다차원 배열

```java
// 자료형[][]배열이름 = new 자료형[행개수][열개수]
int[][]arr = new int[2][3];
// 2행 3열 이차원 배열
int[][]arr = {{1,2,3},{4,5,6}}
```

## ArrayList 클래스 사용하기

객체 배열을 좀 더 쉽게 사용할 수 있도록 ArrayList 클래스를 제공 한다. 객체 배열을 관리할 수 있는 멤버변수와 메서드를 제공하므로 필요한 경우 찾아서 사용한다.

```java
import java.util.ArrayList;
// 기본형식
Array<E> 배열이름 = new ArrayList<E>(); // <E> - 제네릭 자료형
// < >안에 객체의 자료형을 쓰면 된다.

ArrayList<Book> library = new ArrayList<Book> ();
//
```

```java
// ArrayList 클래스 활용
package array;
import java.util.ArrayList; // 자동 import
public class ArrayListTest {
 public static void main(String[] args) {
  // ArrayList 선언
  ArrayList<Book> libraryArrayList = new ArrayList<Book>();
  
  // add() 메서드로 요소 값 추가
  libraryArrayList.add(new Book("author1", "book1"));
  libraryArrayList.add(new Book("author2", "book2"));
  libraryArrayList.add(new Book("author3", "book3"));
  
  // 향산된 for 문으로 배열에 추가된 요소 출력
  for (Book book : libraryArrayList) {
   book.showBookInfo();
  }
 }
}
```

## 배열 응용 프로그램

## 교제 코딩연습

```java
// p.211-1, 객체 배열 만들어 활용하기
package array;
public class Student {
 int studentID;
 String name;
 
 public Student(int studentID, String name) {
  this.studentID = studentID;
  this.name = name;
 }
 
 public void showStudentInfo() {
  System.out.println(studentID + " " + name);
 }
}
```

```java
// p.211-2
package array;
public class StudentArray {
 public static void main(String[] args) {
  Student[] students = new Student[3];
  
  students[0] = new Student(1001, "James");
  students[1] = new Student(1002, "Tomas");
  students[2] = new Student(1003, "Edward");
  
  for(int i=0; i < students.length; i++) {
   students[i].showStudentInfo();
  }
 }
}
```

```java
// p.225
package array;
import java.util.ArrayList;
public class StudentArrayList {
 public static void main(String[] args) {
  ArrayList<Student> student = new ArrayList<Student>();
  
  student.add(new Student(1001, "park") );
  student.add(new Student(1002, "kim") );
  student.add(new Student(1003, "lee") );
  
  for (Student student2 : student) {
   student2.showStudentInfo();
  }
 }
}
```
