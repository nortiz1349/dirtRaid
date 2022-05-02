---
title: "DB 조인"
excerpt: JOIN
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
date: 2022-045-02
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

