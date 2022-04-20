---
title: "Oracle (day2)"
excerpt: SELECT문의 기본 형식, WHERE절 연산자
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - oracle
  - database

toc: true
toc_sticky: true
date: 2022-04-20
---

## SELECT, FROM
```sql
// 테이블 전체 열 조회하기
SELECT * FROM EMP;

// 열을 쉼표로 구분하여 출력하기
SELECT EMPNO, ENAME, DEPTNO
    FROM EMP;

// 테이블 목록 보기
SELECT * FROM TAB;

// 표현식(리터럴)을 사용하여 출력하기
SELECT ENAME, 'GOOD MORNING' AS "good morning"
    FROM EMP;
```
## DESC
```sql
// 테이블 구조 보기
DESC 테이블명;
```


## DISTINCT
```sql
// DISTINCT로 열의 중복 제거하기
SELECT DISTINCT DEPNO
    FROM EMP;

// 여러 개 열을 명시하여 중복 제거하기
SELECT DISTICT JOB, DEPTNO
    FROM EMP;

// 중복되는 열 제거 없이 그대로 출력
SELECT ALL JOB, DEPTNO
    FROM EMP;
```

## 연산식(+,*), 별칭(alias) 설정하기
```sql
// 열에 연산식을 사용하여 출력하기
SELECT ENAME, SAL, SAL*12+COMM, COMM
    FROM EMP;
  // COLUMN명이 SAL*12+COMM으로 표현되고 별칭을 지정할 수 있음
  // 급여(SAL)에 12를 곱하고 수당(COMM)을 더한 값을 출력 -> COMM이 NULL인 경우 SAL*12+COMM 도 NULL로 출력됨

// 별칭을 사용하여 사원의 연간 총 수입 출력하기
SELECT ENAME, SAL, SAL*12+COMM AS ANNSAL, COMM
    FROM EMP;
  // SAL*12+COMM의 별칭을 ANNSAL로 지정함
```

## ORDER BY 출력데이터 정렬
```sql
// EMP 테이블의 모든 열을 급여 기준으로 오름차순 정렬하기
SELECT *
    FROM EMP
  ORDER BY SAL;

// 내림차순 정렬
SELECT *
    FROM EMP
  ORDER BY SAL DESC;
  // ASC(오름차순), DESC(내림차순)
  // 각각의 열에 내림차순, 오름차순 동시에 사용도 가능하다
  // ORDER BY 정렬은 자원을 많이 소모하므로 필요한 경우가 아니라면 사용하지 않는 것이 좋음
```

## WHERE 필요한 데이터만 출력하기
```sql
// 부서번호가 30인 데이터만 출력
SELECT *
    FROM EMP
  WHERE DEPTNO = 30;
```

## AND, OR 조건식
```sql
// AND: 모든 조건을 만족해야 출력
SELECT *
    FROM EMP
  WHERE DEPTNO = 30
    AND JOB = 'SALESMAN';

// OR: 하나만 만족해도 출력
SELECT *
    FROM EMP
  WHERE DEPTNO = 30
    OR JOB = 'CLERK';
```
- 조건식의 개수는 사실상 제한이 없다고 봐도 무방하다.
- 따옴표 구분 `" "`, `' '`
  - SQL "문자" = 칼럼명 "이름"
  - '문자' = 데이터 '문자'

## 대소 비교연산자
```SQL
// 대소 비교연산자를 사용하여 출력
SELECT *
    FROM EMP
  WHERE SAL >= 3000; // 급여가 3000 이상인 경우 출력

// 문자를 비교연산자로 비교
SELECT *
    FROM EMP
  WHERE ENAME >= 'F';
// 열 값의 첫번째 문자가 알파벳 순서상 F와 같거나 뒤에 있는 문자열을 출력
// 알파벳 순서로 '대소'를 비교함
```

## 등가 비교연산자
- 양쪽 값이 같은지 검사
`A != B, A <> B, A ^= B`
-> `A` 와 `B`의 값이 다를 경우 `TRUE`, 같을 경우 `FALSE`
-> 모두 동일한 결과 값을 가진다.
- 논리 부정 연산자 `NOT`과 동일한 출력값을 가진다.
```sql
SELECT *
    FROM EMP;
  WHERE SAL != 3000;
// 열 값이 3000인 행은 출력에서 제외됨
```

## IN, NOT IN 연산자
- IN 연산자를 사용하면 특정 열에 해당하는 조건을 여러개 지정할 수 있다.
```sql
// IN - 모두 해당하는 결과값 출력
SELECT *
    FROM EMP
  WHERE JOB IN ('MANAGER', 'SALESMAN', 'CLERK');

// NOT IN 모두 해당하지 않는 결과값 출력
SELECT *
    FROM *
  WHERE JOB NOT IN ('MANAGER', 'SALESMAN', 'CLERK');
```

## BETWEEN A AND B 연산자
- 최소, 최대값 사이의 결과값을 출력
```sql
SELECT *
    FROM EMP
  WHERE SAL BETWEEN 2000 AND 3000;
// NOT BETWEEN으로 해당 구간을 제외할 수도 있다
```

## LIKE 연산자와 와일드 카드
- LIKE 연산자는 일부 문자열이 포함된 데이터를 조회할 때 사용한다.
```SQL
SELECT *
    FROM EMP
  WHERE ENAME LIKE 'S%';
// ENAME 열 값이 대문자 S로 시작하는 데이터를 조회함
```
###### 와일드카드
- `_` : 어떤 값이든 상관없이 한 개의 데이터를 의미
- `%` : 길이와 상관없이(문자 없는 경우도 포함) 모든 문자 데이터를 의미
```SQL
SELECT *
    FROM EMP
  WHERE ENAME LIKE '_L%';
// 첫번째 문자는 아무거나, 두번째 문자는 L로 시작하는 데이터를 조회함
```
```SQL
SELECT *
    FROM EMP
  WHERE ENAME LIKE '%AM%';
// 사원 이름에 AM이 포함되어 있는 데이터만 출력, NOT LIKE 를 활용할 수도 있음
```
###### ESCAPE절
```SQL
SELECT *
    FROM SOME_TABLE
  WHERE SOME_COLUMN LIKE 'A₩_A%' ESCAPE '₩';
// ₩ 문자 바로 뒤에 있는 `_`는 와일드카드가 아니라 문자로 인식함
```


## IS NULL 연산자
###### NULL
- 현재 무슨 값인지 확정되지 않은 상태
- 값 자체가 존재하지 않는 상태
```SQL
SELECT *
    FROM EMP;
  WHERE COMM IS NULL;
// 수당(COMM) 열 값이 존재하지 않는 데이터 출력
// IS NOT NULL 활용 가능
```

## 집합연산자
###### UNION (합집합)
- `SELECT`문으로 조회한 결과를 하나의 집합과 같이 사용할 수 있는 연산자
```sql
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
  WHERE DEPTNO = 10
UNION
SELECT EMPNO, ENAMEM, SAL, DEPTNO
    FROM EMP
  WHERE DEPTNO = 20;
// 10번과 20번 부서에 근무하는 사원 정보를 합쳐서 출력함
```
- *열 개수와 열의 자료형이 순서별로 일치해야 함*
- `UNION ALL` 연산자는 중복데이터도 모두 출력함.

###### MINUS (차집합)
```SQL
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
MINUS
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
  WHERE DEPTNO = 10;
// 10번 부서에 해당하는 사원 데이터는 제외한 결과 값이 출력됨.
```

###### INTERSECT (합집합))
```SQL
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
INTERSECT
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
  WHERE DEPTNO = 10;
```

