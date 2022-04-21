---
title: "JAVA 개발환경 준비"
excerpt: JDK 설치, JAVA 환경변수 설정, Eclipse
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - java
  - eclipse
  - 초기설정

toc: true
toc_sticky: true
date: 2022-04-20
---


## 1. JDK 설치

  수업에 사용하는 버전은 `jdk-11.0.2`

## 2. JAVA 환경변수 설정(윈도우)

내 맥북에서는 별도로 환경변수 설정을 하지 않았는데도 터미널에서 문제 없이 실행 되었다.

1. `고급 시스템 설정` > `환경변수`
2. 변수이름: `JAVA_HOME`, 변수값: `C:\Program Files\Java\jdk-11.0.2`
3. Path: `%JAVA_HOME%\bin`
4. 변수이름: `CLASSPATH`, 변수값: `%JAVA_HOME%\lib;.;`
5. `java -version` 으로 설치확인

## 3. Eclipse 설치 후 실행

개인별 워크스페이스를 만들고 강사님이 배포한 수업자료를 `import` 하여 불러 들인다.

`import` > `General` > `Projects from Folder...`

## 4. Eclipse Perspective 추가

우측 상단 아이콘(Open Perspective)에서 `java`, `debug` 추가한다.

## 5. 자바 프로젝트 생성

1. `Package Explorer` > `New` > `Java Project`
2. `Project Name` 입력
3. `Location` > 본인이 생성했던 워크스페이스가 맞는지 확인한다.
4. `Java SE 11.0.14` 수업에 사용할 JRE 환경 확인.