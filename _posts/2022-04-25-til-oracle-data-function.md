---
title: "DB 데이터 처리와 가공을 위한 오라클 함수"
excerpt: 문자열, 숫자, 날짜, 형변환, NULL 
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - oracle
  - database
  - sql
  - Do it!

toc: true
toc_sticky: true
date: 2022-04-25
---

## 오라클 함수

`내장함수`

### 1. 문자 함수

- `UPPER(문자열)` : 괄호 안 문자 데이터를 모두 대문자로 변환하여 반환  
- `LOWER(문자열)` : 괄호 안 문자 데이터를 모두 소문자로 변환하여 반환  
- `INTCAP(문자열)` : 첫 글자는 대문자, 나머지는 소문자로 변환

```sql
--- 사원 이름에 SCOTT 단어를 포함한 데이터 찾기
SELECT *
  FROM EMP
WHERE UPPER(ENAME) LIKE UPPER('%scott%');
```

- `LENGTH(문자열)` : 문자열의 길이를 반환  
- `LENGTHB(문자열)` : 문자열의 바이트 수를 반환

```sql
--- 사원 이름의 길이가 5 이상인 행 출력하기
SELECT ENAME, LENGTH(ENAME)
  FROM EMP
WHERE LENGTH(ENAME) >= 5;
```

- `SUBSTR(문자열, 시작 위치, 추출 길이)`  
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

- `INSTR(문자열, 찾는 문자열, 시작위치, 몇번째 문자인지)`  
특정 문자나 문자열이 어디에 포함되어 있는지 찾을 때  

```sql
SELECT INSTR('HELLO, ORACLE!', 'L')       -- 3
      INSTR('HELLO, ORACLE!', 'L', 5)    -- 12
      INSTR('HELLO, ORACLE!', 'L', 2, 2) -- 4
  FROM DUAL;
```

- `REPLACE(문자열, 찾는 문자, 대체할 문자)`  
특정 문자를 다른 문자로 대체할 경우

```sql
SELECT '010-1234-5678'
    REPLACE('010-1234-5678', '-', ' ')  -- 010 1234 5678
    REPLACE('010-1234-5678', '-')       -- 01012345678
  FROM DUAL;
```

- `LPAD(문자열, 자릿수, 채울 문자)` `RPAD(문자열, 자릿수, 채울 문자)`  
데이터의 빈 공간을 특정 문자로 채우는 경우  

```sql
SELECT
    RPAD('971225-', 14, '*')    -- 971225-*******
    RPAD('010-1234-', 13, '*')  -- 010-1234-****
  FROM DUAL;
```

- `CONCAT(문자열, 문자열)`  
문자열 데이터를 합치는 함수

- `TRIM([삭제 옵션(선택)] [삭제할 문자(선택)] FROM [문자열(필수)])`  
- `LTRIM([문자열(필수)], [삭제할 문자(선택)])`  
- `RTRIM([문자열(필수)], [삭제할 문자(선택)])`  
특정 문자를 지우는 함수, 삭제할 문자가 생략되는 경우 공백 제거.  
삭제옵션 - `LEADING` : 왼쪽, `TRAILING` : 오른쪽, `BOTH` : 양쪽

### 2. 숫자 함수

- `ROUND(숫자, 반올림 위치)`  
- `TRUNC(숫자, 버림 위치)`  
- `CEIL(숫자)` : 가까운 큰 정수  
- `FLOOR(숫자)` : 가까운 작은 정수  
- `MOD(숫자, 나눌 숫자)` : 나머지 구하기

### 3. 날짜 함수

- `SYSDATE`  
데이터베이스의 현재 날짜와 시간을 보여준다.

```sql
SELECT SYSDATE AS NOW,
      SYSDATE-1 AS YESTERDAY,
      SYSDATE+1 AS TOMORROW
  FORM DUAL;
```

- `ADD_MONTHS([날짜 데이터], [더할 개월 수(정수)])`

- `MONTHS_BETWEEN([날짜 데이터], [날짜 데이터]`

- `NEXT_DAY([날짜데이터], [요일])`  
날짜 기준으로 돌아오는 요일의 날짜를 출력  

- `LAST_DAY([날짜데이터])`  
특정 날짜가 속한 달의 마지막 날짜를 출력  
  - 날짜에도 반올림, 버림 `ROUND`, `TRUNC` 함수 사용이 가능하다.
  - 날짜 기준 포맷 종류가 많은데, 필요할 때 검색해서 사용.

### 4. 형 변환 함수

- `TO_CHAR([날짜 데이터], 문자 형태)`  
숫자,날짜 > 문자 데이터로 변환

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS') AS 현재날짜시간
  FROM DUAL;
```

- 다양한 형식을 지정하여 출력할 수 있다.

- `TO_NUMBER`  
문자 > 숫자 데이터로 변환

- `TO_DATE`  
문자 > 날짜 데이터로 변환

### 5. NULL 처리 함수

데이터가 NULL이면 산술 연산자나 비교 연산자가 동작하지 않는다. 연산 수행을 위해 NULL 값을 다른 값으로 대체 할 때 사용한다.

- `NVL([문자열],[대체할 값])`  
문자열이 NULL이면 지정한 값으로 변환, NULL이 아니면 그대로 반환.

- `NVL2([문자열], [NULL이 아닌 경우 값], [NULL인 경우 값])`  
NULL이 아닌 경우에 특정한 값을 지정할 수 있다.

### 6. DECODE, CASE

if 조건문 또는 switch-case 조건문과 유사하다.

```sql
DECODE ([검사대상],
        [조건1], [조건1과 일치할 때 반환할 값],
        [조건2], [조건2와 일치할 때 반환할 값],
        ...
        [위 조건과 일치하는 경우가 없을때 반환할 값])

-- 예제
SELECT EMPNO, ENAME, JOB, SAL,
       DECODE(JOB,
            'MANAGER' , SAL*1.1,
            'SALESMAN', SAL*1.05,
            'ANALYST' , SAL,
              SAL*1.03) AS UPSAL
    FROM EMP;
```

- 조건이 일치하지 않을 경우의 값이 없을 경우 NULL이 반환된다.

```sql
CASE ([검사대상],
    WHEN [조건1] THEN [조건1 true일때 반환할 값]
    WHEN [조건2] THEN [조건2 true일때 반환할 값]
    ...
    ELSE [위 조건과 일치하는 경우가 없을 때 반환할 값])

SELECT EMPNO, ENAME, JOB, SAL,
    CASE JOB
        WHEN 'MANAGER' THEN SAL*1.1
        WHEN 'SALESMAN' THEN SAL*1.05
        WHEN 'ANALYST' THEN SAL
        ELSE SAL*1.03
    END AS UPSAL
  FROM EMP;
```

- CASE문은 각 조건식의 true, false 여부만 검사하므로 기준 데이터가 없어도 사용 가능하다.

## 연습문제

```sql
-- Q1
SELECT EMPNO,
      RPAD(SUBSTR(EMPNO,1,2), 4, '*') AS MASKING_EMPNO,
      ENAME,
      RPAD(SUBSTR(ENAME,1,1), 5, '*') AS MASKING_ENAME
    FROM EMP
  WHERE LENGTH(ENAME) = 5;
```

```sql
-- Q2
SELECT EMPNO, ENAME, SAL,
    TRUNC(SAL / 21.5, 2) AS DAY_PAY,
    ROUND(SAL / 21.5 / 8, 1) AS TIME_PAY
  FROM EMP;
```

```sql
-- Q3
SELECT EMPNO, ENAME,
      TO_CHAR(HIREDATE, 'YYYY/MM/DD') AS HIREDATE,
      TO_CHAR(NEXT_DAY(ADD_MONTHS(HIREDATE, 3), '������'), 'YYYY/MM/DD') AS R_JOB,
      NVL(TO_CHAR(COMM), 'N/A') AS COMM
  FROM EMP;
```

```sql
-- Q4
SELECT EMPNO, ENAME,
    NVL(TO_CHAR(MGR), ' ') AS MGR,
    CASE NVL(SUBSTR(MGR,1,2), ' ')
        WHEN ' ' THEN '0000'
        WHEN '75' THEN '5555'
        WHEN '76' THEN '6666'
        WHEN '77' THEN '7777'
        WHEN '78' THEN '8888'
        ELSE TO_CHAR(MGR)
    END AS CHG_MGR
  FROM EMP;
```
