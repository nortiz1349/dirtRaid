---
title: "Oracle (day2)"
excerpt: Do it! 데이터베이스 수업 2일차
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - oracle
  - database
  - TIL

toc: true
toc_sticky: true
date: 2022-04-20
---

## SELECT, FROM
```sql
- 테이블 전체 열 조회하기
  SELECT * FROM EMP;
- 열을 쉼표로 구분하여 출력하기
  SELECT EMPNO, ENAME, DEPTNO
    FROM EMP;
```
## DISTINCT
```sql
- DISTINCT로 열의 중복 제거하기
  SELECT DISTINCT DEPNO
    FROM EMP;
- 여러 개 열을 명시하여 중복 제거하기
  SELECT DISTICT JOB, DEPTNO
    FROM EMP;
- 중복되는 열 제거 없이 그대로 출력
  SELECT ALL JOB, DEPTNO
    FROM EMP;
```
## 연산식(+,*), 별칭(alias) 설정하기
```sql
- 열에 연산식을 사용하여 출력하기
  SELECT ENAME, SAL, SAL*12+COMM, COMM
    FROM EMP;
  - COLUMN명이 SAL*12+COMM으로 표현되고 별칭을 지정할 수 있음.
  - 급여(SAL)에 12를 곱하고 수당(COMM)을 더한 값을 출력 -> COMM이 NULL인 경우 SAL*12+COMM 도 NULL로 출력됨
- 별칭을 사용하여 사원의 연간 총 수입 출력하기
  SELECT ENAME, SAL, SAL*12+COMM AS ANNSAL, COMM
    FROM EMP;
  - SAL*12+COMM의 별칭을 ANNSAL로 지정함.
```
## ORDER BY 출력데이터 정렬
```sql
- EMP 테이블의 모든 열을 급여 기준으로 오름차순 정렬하기
  SELECT *
    FROM EMP
  ORDER BY SAL;
- 내림차순 정렬
  SELECT *
    FROM EMP
  ORDER BY SAL DESC;
  - ASC(오름차순), DESC(내림차순)
  - 각각의 열에 내림차순, 오름차순 동시에 사용도 가능하다.
  - ORDER BY 정렬은 자원을 많이 소모하므로 필요한 경우가 아니라면 사용하지 않는 것이 좋음.
```
## WHERE 필요한 데이터만 출력하기
```sql
- 부서번호가 30인 데이터만 출력
SELECT *
  FROM EMP
WHERE DEPTNO = 30;
```
## AND, OR 조건식
```sql
- AND: 모든 조건을 만족해야 출력
  SELECT *
    FROM EMP
  WHERE DEPTNO = 30
    AND JOB = 'SALESMAN';
- OR: 하나만 만족해도 출력
  SELECT *
    FROM EMP
  WHERE DEPTNO = 30
    OR JOB = 'CLERK';
```
- 조건식의 개수는 사실상 제한이 없다고 봐도 무방하다.