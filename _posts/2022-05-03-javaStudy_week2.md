---
title: "[JAVA STUDY] 자바 데이터 타입, 변수 그리고 배열"
excerpt: 자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법
excerpt_separator: "<!--more-->"
categories:
  - JAVA
tags:
  - JAVA
  - type
  - array
  - Study

toc: true
toc_sticky: true
date: 2022-05-03
---

## 목표

자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익힌다.

## 학습할 것

- 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- 프리미티브 타입과 레퍼런스 타입
- 리터럴
- 변수 선언 및 초기화하는 방법
- 변수의 스코프와 라이프타임
- 타입 변환, 캐스팅 그리고 타입 프로모션
- 1차 및 2차 배열 선언하기
- 타입 추론, var

---

## Java Data Type

자바에서 변수는 특정 데이터 타입으로 지정되어야만 한다.

테이터 타입은 크게 두가지로 나눌 수 있다.

- Primitive data types  
  byte, short, int, long, float, double, boolean, char

- Non-primitive data types  
  String, Arrays and Classes

## Primitive Data Type



|Data|Type Size|표현 번위|Default|
|---|---|---|---|
|byte|1 byte|-128 ~ 127|0|
|short|2 bytes|-32,768 ~ 32,767|0|
|int|4 bytes|-2<sup>31</sup> ~ (2<sup>31</sup> -1)|0|
|long|8 bytes|-2<sup>63</sup> ~ (2<sup>63</sup> -1)|0L|
|float|4 bytes|3.4 x 10<sup>-38</sup> ~ 3.4 x 10<sup>38</sup> 의 근사값|0.0F|
|double|8 bytes|1.7 x 10<sup>-308</sup> ~ 1.7 X 10<sup>308</sup> 의 근사값|0.0|
|boolean|1 bit|true, false|false|
|char|2 bytes|Stores a single character/letter or ASCII values|'\u0000'|

1. byte

메모리 절약이 중요한 대형 배열에서 메모리를 절약 할 때 유용하게 쓰일 수 있다. 그리고 변수의 범위가 제한되어 있다는 사실을 명시적으로 표현하기 위해 쓸 수도 있다.

2. short

byte와 동일한 가이드가 적용된다.

3. int

Java SE 8 버전 이후부터 integer class를 사용하여 int 데이터 타입을 최소값 0, 최대값 (2<sup>32</sup>-1)을 가지는 unsigned integer로 사용할 수 있다. 자세한 내용은 [Number classes](https://docs.oracle.com/javase/tutorial/java/data/numberclasses.html) 섹션을 참고하자.

4. long



5. float
6. double
7. bollean
8. char