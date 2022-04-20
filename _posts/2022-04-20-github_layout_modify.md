---
title: "Github page 폰트, 레이아웃"
excerpt: minimal-mistakes 폰트크기 조절, 좌우 네비게이션 width 조정
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - gitHub page
  - minimal mistakes

toc: true
toc_sticky: true
date: 2022-04-20
---
## 폰트 사이즈 조정

`/_sass/minimal-mistakes/_reset.scss`

위 파일을 찾아 아래 코드를 찾는다.
```css
html {
  /* apply a natural box layout model to all elements */
  box-sizing: border-box;
  background-color: $background-color;
  font-size: 16px;

  @include breakpoint($medium) {
    font-size: 18px;
  }

  @include breakpoint($large) {
    font-size: 20px;
  }

  @include breakpoint($x-large) {
    font-size: 20px;
  }

  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
}
```
해당 테마는 사용자 환경에 따른 유동적 폰트 크기를 지원하기 위해 `breakpoint` 라는 개념을 사용한다. 모바일에서부터 해상도가 높아질수록 폰트가 커지게 default 값으로 설정되어 있는 듯 하다. 

아래는 현재 페이지의 폰트 사이즈 설정.

```css
html {
  /* apply a natural box layout model to all elements */
  box-sizing: border-box;
  background-color: $background-color;
  font-size: 14px;

  @include breakpoint($medium) {
    font-size: 16px;
  }

  @include breakpoint($large) {
    font-size: 16px;
  }

  @include breakpoint($x-large) {
    font-size: 16px;
  }

  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
}
```