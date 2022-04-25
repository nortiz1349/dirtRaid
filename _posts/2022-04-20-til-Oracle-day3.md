---
title: "데이터 처리와 가공을 위한 오라클 함수"
excerpt: 오라클 수업 - day 3
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - oracle
  - database
  - sql

toc: true
toc_sticky: true
date: 2022-04-20
---

## 오라클 함수

`내장함수`

### 1. 문자 함수

`UPPER(문자열)` : 괄호 안 문자 데이터를 모두 대문자로 변환하여 반환  
`LOWER(문자열)` : 괄호 안 문자 데이터를 모두 소문자로 변환하여 반환  
`INTCAP(문자열)` : 첫 글자는 대문자, 나머지는 소문자로 변환  

  ```sql
  --- 사원 이름에 SCOTT 단어를 포함한 데이터 찾기
  SELECT *
    FROM EMP
  WHERE UPPER(ENAME) LIKE UPPER('%scott%');
```

`LENGTH(문자열)` : 문자열의 길이를 반환  
`LENGTHB(문자열)` : 문자열의 바이트 수를 반환

  ```sql
  --- 사원 이름의 길이가 5 이상인 행 출력하기
  SELECT ENAME, LENGTH(ENAME)
    FROM EMP
  WHERE LENGTH(ENAME) >= 5;
```

`SUBSTR(문자열, 시작위치, 추출길이)`  
  문자열 데이터의 시작 위치부터 추출 길이만큼 추출.  
  시작위치가 `음수`일 경우 문자열 마지막에서 거슬러 올라간 위치에서 시작.  
  추출 길이를 지정하지 않을 경우 문자열 데이터 끝까지 추출.

  ```sql
  SELECT JOB,
    SUBSTR(JOB, -LENGTH(JOB)),    -- 문자열 처음부터 끝까지 출력
    SUBSTR(JOB, -LENGTH(JOB), 2), -- 문자열 처음부터 2자리 출력
    SUBSTR(JOB, -3)               -- 문자열 뒤에서 3자리부터 끝까지 출력
  FROM EMP;
  ```

`INSTR`
