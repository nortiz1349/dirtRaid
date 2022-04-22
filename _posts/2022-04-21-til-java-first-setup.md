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
date: 2022-04-21
---


## 1. JDK 설치

  수업에 사용하는 버전은 `jdk-11.0.14`

## 2. JAVA 환경변수 설정

자바가 설치된 디렉토리로 이동 하지 않고도 전역적으로 터미널이나 커맨드창에서 컴파일이 가능해진다. 그리고 라이브러리 관리 시스템인 MAVEN에서 JAVA_HOME 환경변수를 찾는 경우가 있다고 한다. 내 맥북에서는 별도로 설정을 하지 않았는데도 터미널에서 문제 없이 실행 되었다. 윈도우인 경우 아래와 같이 설정하면 된다.

1. `고급 시스템 설정` > `환경변수` > `시스템변수` > `새로만들기`
2. 변수이름 : `JAVA_HOME`, 변수값 : `C:\Program Files\Java\jdk-11.0.14` (자바가 설치된 경로)
3. 시스템 변수 `Path` 편집 > `%JAVA_HOME%\bin` 추가
4. 변수이름: `CLASSPATH`, 변수값: `%JAVA_HOME%\lib;.;`
5. `cmd` > `java -version` 으로 설치확인

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

## 6. Package, Class 생성

- `Package`: 다양한 자바 파일들을 관리하기 위한 폴더라고 생각하면 된다.

1. `src` 폴더 > `New` > `Package`
2. 해당 패키지 폴더에 `Class` 생성

   - 클래스 이름은 대문자로 시작한다.
   - 변수, 매서드 이름은 소문자로 시작, 단어가 이어지는 부분은 대문자
   - `public static void main(String[] args)` 당분간 체크할 것

3. 아래 코드 작성

```java
package ga.nortizbitc.exs;

public class Ex1_220421 {

 public static void main(String[] args) {
  System.out.println("안녕 자바야!");
  // 실행은 ctrl+f11, 맥은 fn+cmd+f11
 }

}
```

## 7. 자동완성 활성화

1. `Search` > `Content Assist - JAVA/Editor`
2. `Auto Activation trigger for JAVA` 에 아래 문자열 입력
3. `<=$:{.@qwertyuioplkjhgfdsazxcvbnm_QWERTYUIOPLKJHGFDSAZXCVBNM`

## 8. 단축키

- 인라인 주석 : `cmd` + `/`
- 블록 주석 : `fn` + `cmd` + `/`
- Javadoc 보기 : `shift` + `fn` + `f2`
- 실행 : `fn` + `cmd` + `f11`
- 코드 입력창 확장 : `ctrl` + `m`

## 9. 이클립스 디버깅

1. 소스 코드 창의 라인 번호를 더블 클릭하여 `중단점(break point)` 시작 부분과 끝나는 부분 2개를 설정한다.
2. 이클립스 오른쪽 상단의 `debug` 모드로 전환. 소스의 특정 부분을 검사하거나, 메모리 구조를 보는데 사용한다.
3. debug 실행
4. 처음 설정한 시작 지점이 하이라이트 되어 있다.
  
- `Step into` > 매서드, 반복문, 특정 부분에 들어갈 때 사용
- `Step over` > 다음 단계로 진행

## 10. 맥북 이클립스 한글 깨짐

윈도우에서 작업한 파일을 맥북에서 `import` 하니 한글로 작성한 주석 부분이 깨지는 현상 발생. 윈도우 이클립스 기본 인코딩 설정인 `MS949` 로 수정하면 정상적으로 한글이 표시 된다.

1. `import`한 프로젝트의 `Properties` > `Resource`

2. `Text file encoding` > `Other` > `MS949` 선택

> 워크스페이스의 전역 인코딩 설정을 바꾸었더니 기존 UTF-8로 인코딩 되었던 한글이 다시 깨지는 상황이 발생하였다ㅠ 개별 프로젝트 단위로 설정을 조절하는 것이 좋을듯 하다.

## 11. debug 모드 Source

JRE library
