---
title: "DB 서브쿼리"
excerpt: subquery
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
date: 2022-05-02
---

## 서브쿼리

쿼리 안에 쿼리를 사용하는 구문 : SQL문을 실행하는데 필요한 데이터를 추가로 조회 하기 위해 SQL문 내부에서 사용하는 SELECT문을 의미한다.

```sql
SELECT COL  -- 메인쿼리
  FROM TABLE 
  WHERE 조건식 (SELECT COL  -- 서브쿼리
              FROM TABLE
              WHERE 조건식)
-- 서브쿼리로 JONES의 급여보다 높은 급여를 받는 사원 정보 출력하기
SELECT *
    FROM EMP
  WHERE SAL > (SELECT SAL
                  FROM EMP
                WHERE ENAME = 'JONES');
```

- 서브쿼리는 연산자와 같은 비교 또는 조회 대상의 오른쪽에 놓이며 괄호 `()`로 묶어서 사용한다.
- 특수한 몇몇 경우를 제외한 대부분의 서브쿼리에서는 `ORDER BY`절을 사용할 수 없다.
- 서브쿼리의 `SELECT` 절에 명시한 열은 메인쿼리의 비교 대상과 같은 자료형과 같은 개수로 지정해야 한다. 즉 메인쿼리의 비교 대상 데이터가 하나라면 서브쿼리의 `SELECT`절 역시 같은 자료형인 열을 하나 지정해야 한다.
- 서브쿼리에 있는 `SELECT`문의 결과 행 수는 사용하는 메인쿼리의 연산자 종류와 호환 가능해야 한다.

## 단일행 서브쿼리

메인쿼리와 서브쿼리 결과를 단일행 연산자를 사용하여 비교하고 실행 결과가 단 하나의 행으로 나오게 된다.

## 다중행 서브쿼리 (결과가 여러개)

다중행 연산자를 사용하여 실행 결과 행이 여러개로 나오는 서브쿼리.

### 1) IN

다중행 쿼리의 결과가 일치하는 항목만 출력한다.

```sql
-- 각 부서별 최고 급여를 받는 사원
SELECT *
    FROM EMP
  WHERE SAL IN (SELECT MAX(SAL)   -- 부서별 최고급여를 구하고
                  FROM EMP        -- 이 데이터와 일치하는 메인쿼리 데이터를 IN 연산자로 선별함.
                GROUP BY DEPTNO);
```

### 2) ANY, SOME

서브쿼리 결과 중 하나만 맞더라도 메인 쿼리문의 결과를 출력한다.

```sql
-- 30번 부서 사원들의 최대급여보다 적은 급여를 받는 사원 출력
SELECT *
    FROM EMP
  WHERE SAL < ANY (SELECT SAL  
                      FROM EMP       
                    WHERE DEPTNO = 30)
ORDER BY SAL, EMPNO;
```

### 3) ALL

서브쿼리의 모든 결과가 조건식에 맞아야지만 출력한다.

```sql
-- 부서 번호가 30번인 사원들의 최소 급여보다 더 적은 급여를 받는 사원
SELECT *
    FROM EMP
  WHERE SAL < ALL (SELECT SAL
                      FROM EMP
                    WHERE DEPTNO = 30);
```

### 4) EXISTS

서브쿼리의 결과 값이 하나 이상 존재하면 모두 출력, 존재하지 않는다면 메인쿼리 실행이 되지 않는다.

```sql
-- TRUE - 모두 출력됨
SELECT *
    FROM EMP
  WHERE EXISTS (SELECT DNAME
                    FROM DEPT
                  WHERE DEPTNO = 10);
```

## 다중열 서브쿼리 (비교열이 여러개)

```sql
-- 각 부서에서 연봉이 가장 높은 사원 출력
SELECT *
    FROM EMP
  WHERE (DEPTNO, SAL) IN (SELECT DEPTNO, MAX(SAL)
                              FROM EMP
                            GROUP BY DEPTNO);
```

## FROM절에 사용하는 서브쿼리와 WITH절

`인라인 뷰`라고도 부르며 특정 테이블 전체 데이터가 아닌 `SELECT`문을 통해 일부 데이터를 먼저 추출해 온 후 별칭을 주어 사용할 수 있다.

```sql
WITH
E10 AS (SELECT * FROM EMP WHERE DEPTNO =10),
D   AS (SELECT * FROM DEPT)
SELECT E10.EMPNO, E10.ENAME, E10.DEPTNO, D.DNAME, D.LOC
FROM   E10, D
WHERE  E10.DEPTNO = D.DEPTNO;
```

## SELECT절에 사용하는 서브쿼리

`스칼라 서브쿼리`라고도 부른다. SELECT절에 하나의 열 영역으로서 결과를 출력할 수 있다.

```sql
SELECT EMPNO, ENAME, JOB, SAL,
      (SELECT GRADE
          FROM SALGRADE
        WHERE E.SAL BETWEEN LOSAL AND HISAL) AS SALGRADE,
      DEPTNO,
      (SELECT DNAME
          FROM DEPT
        WHERE E.DEPTNO = DEPT.DEPTNO) AS DNAME
  FROM EMP E;
```
