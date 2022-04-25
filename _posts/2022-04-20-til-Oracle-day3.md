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
date: 2022-04-25
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
: 문자열 데이터의 시작 위치부터 추출 길이만큼 추출.  
: 시작위치가 `음수`일 경우 문자열 마지막에서 거슬러 올라간 위치에서 시작.  
: 추출 길이를 지정하지 않을 경우 문자열 데이터 끝까지 추출.

  ```sql
  SELECT JOB,
    SUBSTR(JOB, -LENGTH(JOB)),    -- 문자열 처음부터 끝까지 출력
    SUBSTR(JOB, -LENGTH(JOB), 2), -- 문자열 처음부터 2자리 출력
    SUBSTR(JOB, -3)               -- 문자열 뒤에서 3자리부터 끝까지 출력
  FROM EMP;
  ```

`INSTR(문자열, 찾는 문자열, 시작위치, 몇번째 문자인지)`  
: 특정 문자나 문자열이 어디에 포함되어 있는지 찾을 때  

  ```sql
  SELECT INSTR('HELLO, ORACLE!', 'L')       -- 3
        INSTR('HELLO, ORACLE!', 'L', 5)    -- 12
        INSTR('HELLO, ORACLE!', 'L', 2, 2) -- 4
    FROM DUAL;
  ```

`REPLACE(문자열, 찾는 문자, 대체할 문자)`  
: 특정 문자를 다른 문자로 대체할 경우

  ```sql
  SELECT '010-1234-5678'
      REPLACE('010-1234-5678', '-', ' ')  -- 010 1234 5678
      REPLACE('010-1234-5678', '-')       -- 01012345678
    FROM DUAL;
  ```

`LPAD(문자열, 자릿수, 채울 문자)` `RPAD(문자열, 자릿수, 채울 문자)`  
: 데이터의 빈 공간을 특정 문자로 채우는 경우  

  ```sql
  SELECT
      RPAD('971225-', 14, '*')    -- 971225-*******
      RPAD('010-1234-', 13, '*')  -- 010-1234-****
    FROM DUAL;
  ```

`CONCAT(문자열, 문자열)`  
: 문자열 데이터를 합치는 함수

`TRIM([삭제 옵션(선택)] [삭제할 문자(선택)] FROM [문자열(필수)])`  
`LTRIM([문자열(필수)], [삭제할 문자(선택)])`  
`RTRIM([문자열(필수)], [삭제할 문자(선택)])`  
: 특정 문자를 지우는 함수, 삭제할 문자가 생략되는 경우 공백 제거.  
삭제옵션 - `LEADING` : 왼쪽, `TRAILING` : 오른쪽, `BOTH` : 양쪽

### 2. 숫자 함수

`ROUND(숫자, 반올림 위치)`  
`TRUNC(숫자, 버림 위치)`  
`CEIL(숫자)` : 가까운 큰 정수  
`FLOOR(숫자)` : 가까운 작은 정수  
`MOD(숫자, 나눌 숫자)` : 나머지 구하기

### 3. 날짜 함수

`SYSDATE` : 데이터베이스의 현재 날짜와 시간을 보여준다.

  ```sql
  SELECT SYSDATE AS NOW,
        SYSDATE-1 AS YESTERDAY,
        SYSDATE+1 AS TOMORROW
    FORM DUAL;
  ```

`ADD_MONTHS([날짜 데이터], [더할 개월 수(정수)])`

`MONTHS_BETWEEN([날짜 데이터], [날짜 데이터]`

`NEXT_DAY([날짜데이터], [요일])` : 날짜 기준으로 돌아오는 요일의 날짜를 출력  
`LAST_DAY([날짜데이터])` : 특정 날짜가 속한 달의 마지막 날짜를 출력

- 날짜에도 반올림, 버림 `ROUND`, `TRUNC` 함수 사용이 가능하다.

### 4. 형 변환 함수
