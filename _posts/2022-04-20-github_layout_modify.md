---
title: "Github page 테마 수정 - minimal-mistakes"
excerpt: 폰트크기 수정, 좌우 네비게이션 width 수정, 제목 밑줄 삭제, 폰트 변경
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

`_sass/minimal-mistakes/_reset.scss`

위 파일을 찾아 아래 코드를 찾는다.

```scss
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

```scss
html {
  /* apply a natural box layout model to all elements */
  box-sizing: border-box;
  background-color: $background-color;
  font-size: 16px;

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

## 네비게이션 width 조정

`_sass\minimal-mistakes\_variables.scss`

위 파일을 찾아 아래 코드를 수정 해준다. 디폴트 값을 주석으로 달아 놓았다.

```scss
/*
   Grid
   ========================================================================== */

$right-sidebar-width-narrow: 150px !default; // default 200px
$right-sidebar-width: 250px !default; // default 300px
$right-sidebar-width-wide: 300px !default; // default 400px
```

## 포스트 제목 밑줄 삭제

`_sass\minimal-mistakes\_reset.scss`

아래 코드 한줄만 추가하면 된다.

```scss
/* links */

a {
  text-decoration: none; /* 추가된 코드입니다. */

  &:focus {
    @extend %tab-focus;
  }

  &:visited {
    color: $link-color-visited;
  }

  &:hover {
    color: $link-color-hover;
    outline: 0;
  }
}
```

## 폰트 변경

- 구글 웹 폰트에서 임포트 하기 (<https://fonts.google.com/>)

`assets\css\main.scss`  

```scss
// 구글 웹 폰트 임포트 - 나눔고딕코딩
@import url("https://fonts.googleapis.com/css2?family=Nanum+Gothic+Coding&display=swap");
```

- 폰트 적용하기

`_sass\minimal-mistakes\_variables.scss`

```scss
/* system typefaces */
$serif: Georgia, Times, serif !default;
$sans-serif: -apple-system, BlinkMacSystemFont, "Nanum Gothic Coding", "Roboto",
  "Segoe UI", "Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
$monospace: Monaco, Consolas, "Lucida Console", monospace !default;

// "Nanum Gothic Coding" 추가
```
