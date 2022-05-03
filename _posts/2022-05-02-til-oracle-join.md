---
title: "DB 조인"
excerpt: JOIN
excerpt_separator: "<!--more-->"
categories:
  - DATABASE
tags:
  - oracle
  - sql
  - Do it!
  - TIL

toc: true
toc_sticky: true
date: 2022-05-02
---

## 조인 JOIN

관계형 데이터베이스는 여러 종류의 데이터가 다양한 테이블에 나뉘어 저장되는 특성이 있다. 그래서 실무에서 SQL 문은 단일테이블 조회보다는 여러 테이블의 데이터를 조합하여 출력하는 경우가 많다. 이것을 가능하게 해주는 조회 방식이 조인이다. 조인은 두 개 이상의 테이블을 연결하여 하나의 테이블처럼 출력할 때 사용한다.

지금까지는 FROM절에 EMP 테이블 하나만 명시 했지만 여러 개 테이블을 지정하는 것이 가능하다.

```sql
SELECT *
  FROM EMP, DEPT 
ORDER BY EMPNO;
-- FROM 절에 명시한 각 테이블을 구성하는 행이 모든 경우의 수로 조합되어 출력되기 때문에 데이터가 많아지고 데이터 연결도 부정확하다.
```

- 열 이름을 비교하는 조건식으로 조인하기

```sql
-- 열 앞에 테이블 이름을 명시하여 특정 열이 어느 테이블에 속한 열인지를 구별한다.
SELECT *
    FROM EMP, DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO
ORDER BY EMPNO;
```

- 테이블의 별칭 설정

```sql
SELECT *
    FROM EMP E, DEPT D
  WHERE E.DEPTNO = D.DEPTNO
ORDER BY EMPNO;
```

## 조인 종류

### 1) 등가 조인

앞선 예제에서 사용했던 방식으로 테이블을 연결한 후에 출력 행을 각 테이블의 특정 열에 일치한 데이터를 기준으로 선정하는 방식이다. 내부조인, 단순조인으로도 불린다.

조인 조건이 되는 각 테이블의 열 이름이 같을 경우에 테이블 구분 없이 명시하면 오류가 발생한다.

```sql
-- AS IS (오류발생)
SELECT EMPNO, ENAME, DEPTNO, DNAME, LOC -- DEPTNO 열은 두 테이블에 모두 존재한다.
    FROM EMP E, DEPT D
  WHERE E.DEPTNO = D.DEPTNO;

-- TO BE
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DNAME, D.LOC
  FROM EMP E, DETP D
 WHERE E.DEPTNO = D.DEPTNO
ORDER BY D.DEPTNO, E.EMPNO;
```

- `WHERE`절에 조건식 추가하여 출력 범위 설정

```sql
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DNAME, D.LOC
  FROM EMP E, DETP D
 WHERE E.DEPTNO = D.DEPTNO
  AND SAL >= 3000; -- SAL 3000 이상 출력
```

조인 조건을 제대로 지정하지 않으면 데카르트 곱 때문에 정확히 연결되지 않아 필요없는 데이터가 출력되는 문제가 있다. 이 문제를 일어나지 않게 하기 위한 조건식의 최소 개수는 조인 테이블 개수에서 하나를 뺀 값이다.

테이블 `A`,`B`,`C` 3개인 경우  
> 최소 `(A-B 조인)` 1개, `(A-B 조인) - C 조인` 1개가 필요하다.

### 2) 비등가 조인

```sql
-- BETWEEN A AND B를 활용
SELECT *
    FROM EMP E, SALGRADE S
  WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

### 3) 자체 조인

하나의 테이블을 여러 개의 테이블처럼 활용하여 조인하는 방식이다. FROM절에 같은 테이블을 여러 번 명시하되 테이블의 별칠만 다르게 지정하는 방식으로 사용한다.

```sql
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
    FROM EMP E1, E2
  WHERE E1.MGR = E2.EMPNO;
```

### 4) 외부 조인 (아우터 조인 - OUTER JOIN)

등가 조인, 자체 조인은 데이터가 있는 경우에만 결과를 출력하므로 내부 조인이라고 부른다. 외부 조인은 기준 열의 어느 한쪽이 NULL이어도 강제로 출력하는 방식이다.

- 왼쪽 외부 조인(Left Outer Join) : 왼쪽 열 기준으로 오른쪽 열의 데이터 존재 여부에 상관없이 출력한다.  
`WHERE TABLE1.COL1 = TABLE2.COL1(+)`  
- 오른쪽 외부 조인(Right Outer Join)  
`WHERE TABLE1.COL1(+) = TABLE2.COL1`

```sql
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
    FROM EMP E1, E2
  WHERE E1.MGR = E2.EMPNO(+) -- 왼쪽 조인
ORDER BY E1.EMPNO;
```

## SQL-99 표준 문법으로 배우는 조인

### 1) NATURAL JOIN

등가 조인을 대신하여 사용할 수 있는 방법이다. 조인 대상이 되는 두 테이블에 이름과 자료형이 같은 열을 찾은 후 그 열을 기준으로 등가 조인을 한다.

기존 등가 조인과 다르게 조인 기준이 되는 열을 SELECT 절에 명시할 때는 테이블 이름을 붙이면 안된다.  
> `D.DEPTNO` (X), `DEPTNO` (O)`

### 2) JOIN ~ USING

`NATURAL JOIN`이 자동으로 조인 기준 열을 지정하는 것과 달리 `USING` 키워드에 조인 기준으로 사용할 열을 명시하여 사용한다.

`FROM TABLE1 JOIN TABLE2 USE (조인에 사용한 기준열)`

```sql
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       DEPTNO, D.DNAME, D.LOC
    FROM EMP E JOIN DEPT D USING (DEPTNO)
  WHERE SAL >= 3000
ORDER BY DEPTNO, E.EMPNO;
```

### 3) JOIN ~ ON

WHEREW 절에 있는 조인 조건식을 ON 키워드 옆에 작성한다.

`FROM TABLE1 JOIN TABLE2 ON (조인 조건식)`

### 4) OUTER ~ JOIN

```sql
-- 왼쪽 외부 조인
WHERE TABLE1.COL1 = TABLE2.COL1(+)
FROM TABLE1 LEFT OUTER JOIN TABLE2 ON (조인조건식)
-- 오른쪽 외부 조인
WHERE TABLE1.COL1(+) = TABLE2.COL1
FROM TABLE1 RIGHT OUTER JOIN TABLE2 ON (조인조건식)
-- 전체 외부 조인
FROM TABLE1 FULL OUTER JOIN TABLE2 ON (조인조건식)
```

### 5) 세 개 이상의 테이블을 조인할 때

```sql
FROM TABLE1, TABLE2, TABLE3
WHERE TABLE1.COL = TABLE2.COL
AND TABLE2.COL = TABLR3.COL

-- =

FROM TABLE1 JOIN TABLE2 ON (조건식)
JOIN TABLE3 ON (조건식)
```
