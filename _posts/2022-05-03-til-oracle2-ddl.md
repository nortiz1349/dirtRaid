---
title: "DB 데이터 정의어"
excerpt: DDL, CREATE, ALTER, RENAME, TRUNCATE, DROP
excerpt_separator: "<!--more-->"
categories:
  - TIL
tags:
  - oracle
  - database
  - sql
  - Do it!
  - ddl

toc: true
toc_sticky: true
date: 2022-05-03
---

## 객체를 생성, 변경, 삭제하는 데이터 정의어

데이터 정의어는 DML과 달리 명령어를 수행하자마자 데이터베이스에 수행한 내용이 바로 반영되는 특성이 있다. 즉 DDL을 실행하면 자동으로 `COMMIT` 되기 때문에 영구히 데이터베이스에 반영되게 된다. `ROLLBACK`이 불가하기 때문에 사용시 주의를 기울여야 한다.

## CREATE

```sql
-- 기본형식
CREATE TABLE 소유계정.테이블이름 (
  열1 이름 열1 자료형,
  열2 이름 열2 자료형,
  ...
  열N 이름 열N 자료형
);
```

- 테이블 이름 생성 규칙

  1) 문자로 시작해야 한다.  
  2) 30byte 이하여야 한다.  
  3) 같은 사용자 소유의 테이블 이름은 중복될 수 없다.  
  4) 영문자, 숫자, 특수문자 $, #, _ 를 사용할 수 있다.  
  5) SQL 키워드는 테이블 이름으로 사용할 수 없다.

- 열 이름 생성 규칙

  1) 문자로 시작해야 한다.  
  2) 30byte 이하여야 한다.  
  3) 한 테이블의 열 이름은 중복될 수 없다.  
  4) 영문자, 숫자, 특수문자 $, #, _ 를 사용할 수 있다.  
  5) SQL 키워드는 열 이름으로 사용할 수 없다.

```sql
-- 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE DEPT_DDL
  AS SELECT * FROM DEPT -- 전체 복사
```

```sql
-- 다른 테이블의 일부를 복사하여 테이블 생성하기
CREATE TABLE EMP_DDL_30
  AS SELECT *
    FROM EMP
  WHERE DEPTNO = 30; -- 30번 부서 사원 데이터만 복사하여 저장
```

```sql
-- 기존 테이블의 열 구조만 복사하여 테이블 생성하기
-- WHERE절 조건식이 언제나 false가 나오게 하는 방법을 활용
CREATE TABLE EMPDEPT_DDL
  AS SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE,
            E.SAL, E.COMM, D.DEPTNO, D.DNAME, D.LOC
       FROM EMP E, DEPT D
      WHERE 1 <> 1; -- 언제나 false
```

## ALTER

테이블에 새 열을 추가, 삭제하거나 열의 자료형 또는 길이를 변견하는 등 테이블 구조 변경과 관련된 기능을 수행한다.

- ADD : 열 추가

```sql
ALTER TABLE EMP_ALTER
  ADD HP VARCHAR2(20);
```

- RENAME : 열 이름 변경

```sql
ALTER TABLE EMP_ALTER
  RENAME COLUMN HP TO TEL;
```

- MODIFY : 열 자료형 변경. 기존에 저장되어 있는 데이터가 영향을 받지 않는 범위에서 가능하다.

```sql
ALTER TABLE EMP_ALTER
  MODIFY EMPNO NUMBER(5);
```

- DROP : 열 삭제

```sql
ALTER TABLE EMP_ALTER
  DROP COLUMN TEL;
```

## RENAME

테이블 이름을 변경할 때 사용한다.

```sql
RENAME EMP_ALTER TO EMP_RENAME;
```

## TRUNCATE

특정 테이블의 모든 데이터를 삭제한다. 데이터만 삭제하므로 테이블 구조에는 영향을 주지 않는다. ROLLBACK 되지 않으므로 주의가 필요하다.

```sql
TRUNCATE TABLE EMP_RENAME;
```

## DROP

데이터베이스 객체를 삭제한다. 테이블이 삭제되므로 데이터도 모두 삭제된다.

```sql
DROP TABLE EMP_RENAME;
```

- 오라클 10g부터 윈도우의 휴지통 기능과 같은 `FLASHBACK` 명령을 사용하여 `DROP`으로 삭제된 테이블을 복구할 수 있다.
