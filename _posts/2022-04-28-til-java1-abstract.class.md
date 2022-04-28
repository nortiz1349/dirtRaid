---
title: "JAVA 추상 클래스"
excerpt: abstract class, template method, final
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - java
  - eclipse
  - Do it!

toc: true
toc_sticky: true
date: 2022-04-28
---

## 추상 클래스 `abstract`

추상 클래스는 상속을 하기 위해 만드는 클래스이다. 하위 클래스가 어떤 클래스냐에 따라 구현 코드가 달라지는 경우, 실제 기능이 없는 추상메서드를 선언만 하고 하위 클래스에 구현을 위임한다.

추상 클래스는 구현된 코드가 없으므로 인스턴스로 생성할 수 없지만 형변환은 사용할 수 있다.

```java
// 일반적인 메서드
int add(int x, int y) {
  return x + y;
}
// 추상 메서드. abstract로 메서드를 선언만 함.
abstract int add(int x, int y);
```

- 추상 클래스를 사용하지 않고 빈 블럭으로 메서드를 구현해도 문제는 없다. 다만, 추상 클래스로 선언한 매서드는 상속 받은 하위 클래스에서 필수로 재정의하여 사용야 하므로 추상 클래스로 선언하게 되면 메서드 구현을 강제할 수 있다.

- 강제성과 별개로 `abstract`의 사용은 메서드 구현의 확장 의미로 생각해도 된다.

## 템플릿 메서드

## final
