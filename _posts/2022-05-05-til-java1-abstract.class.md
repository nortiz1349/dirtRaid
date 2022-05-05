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

![image](https://github.com/nortiz1349/dirtraid/blob/master/assets/images/abstract.jpg)

## 템플릿 메서드

## final
