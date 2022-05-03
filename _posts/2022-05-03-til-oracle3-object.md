---
title: "DB 객체 종류"
excerpt: Data Dictionary, Index, View, Sequence, Synonym
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
date: 2022-05-03
---

## 데이터 사전 Data Dictionary

오라클 데이터베이스 테이블은 `사용자 테이블(user table)`과 `데이터 사전(data dictionary)`으로 나누어진다.

- 사용자 테이블 : 데이터베이스를 통해 관리할 데이터를 저장하는 테이블
- 데이터 사전 : 데이터베이스를 구성하고 운영하는 데 필요한 모든 정보를 저장하는 특수한 테이블로 데이터베이스가 생성되는 시점에 자동으로 만들어진다.  

데이터 사전에는 데이터베이스 메모리, 성능, 사용자, 권한, 객체 등 오라클 데이터베이스 운영에 중요한 데이터가 보관되어 있고, 여기에 문제가 생기면 오라클 데이터베이스 사용이 불가능해질수도 있다.

따라서 오라클 데이터베이스는 사용자가 데이터 사전 정보에 직접 접근하거나 작업하는 것을 허용하지 않는다. 그 대신 `데이터 사전 뷰(data dictionary view)`를 제공하여 `SELECT` 문으로 정보 열람이 가능게 해 두 었다.

데이터 사전 뷰는 용도에 따라 접두어를 지정하여 분류한다.

- `USER_XXXX` : 현재 데이터베이스에 접속한 사용자가 소유한 객체 정보
- `ALL_XXXX` : 현재 데이터베이스에 접속한 사용자가 소유한 객체 또는 다른 사용자가 소유한 객체 중 사용 허가를 받은 객체, 즉 사용 가능한 모든 객체 정보
- `DBA_XXXX` : 데이터베이스 관리를 위한 정보(데이터배이스 관리 권한을 가진 SYSTEM, SYS 사용자만 열람 가능)
- `V$_XXXX` : 데이터베이스 성능 관련 정보

```sql
-- SCOTT 계정에서 사용 가능한 데이터 사전 살펴보기
SELECT * FROM DICT;
```

```sql
-- 계정이 가지고 있는 객체 정보 살펴보기
SELECT TABLE_NAME
  FROM USER_TABLES;
```

```sql
-- 사용할 수 있는 객체 정보 살펴보기
SELECT OWNER, TABLE_NAME
  FROM ALL_TABLES;

```

## 인덱스 Index

- FULL SCAN : 처음부터 끝까지 검색하는 방식
- INDEX SCAN : 특정 지점의 인덱스를 통해 데이터를 찾는 방식

```sql
--- 계정이 소유한 인덱스 정보 알아보기
SELECT *
  FROM USER_INDEXES;
```

인덱스는 사용자가 직접 특정 테이블의 열에 지정할 수도 있지만 열이 기본키(primary key) 또는 고유키(unique key) 일 경우에 자동으로 생성된다.

### 인덱스 생성

```sql
CREATE INDEX 인덱스이름
  ON 테이블이름(열 이름1 ASC OR DESC,
             열 이름2 ASC OR DESC,
             ...                );

-- EMP 테이블의 SAL열에 인덱스 생성하기
CREATE INDEX IDX_EMP_SAL
    ON EMP(SAL);

-- 생성된 인덱스 살펴보기
SELECT * FROM USER_IND_COLUMNS;
```

인덱스를 지정할 열의 선정은 데이터의 구조 및 데이터의 분포도 등 여러조건을 고려해서 이루어져야 한다. 인덱스를 지정하면 데이터 조회를 반드시 빠르게 한다고 보장하기는 어렵다.

### 인덱스 삭제

```sql
DROP INDEX 인덱스이름;
```

## 뷰 View

흔히 가상 테이블(virtual table)로 부르는 뷰(view)는 하나 이상의 테이블을 조회하는 SELECT문을 저장한 객체를 의미한다. 물리적 데이터를 따로 저장하는 것이 아니므로, 편리성, 보안성의 목적을 가진다.

- 편리성 : SELECT문의 복잡도를 완화하기 위해.
- 보안성 : 테이블의 특정 열을 노출하고 싶지 않을 경우

### 뷰 생성

```sql
CREATE [OR REPLACE] [FORCE | NOFORCE] VIEW 뷰이름 (열이름1, 열이름2, ...)
    AS (저장할SELECT문)
[WITH CHECK OPTION [CONSTRAINT 제약조건]]
[WITH READ ONLY [CONSTRAINT 제약조건]];
```

1. `OR REPLACE` : 같은 이름의 뷰가 이미 존재할 경우 현재 생성할 뷰로 대체하여 생성(선택)
2. `FORCE` : 뷰가 저장할 SELECT문의 기반 테이블이 존재하지 않아도 강제로 생성(선택)
3. `NO FORCE` : 뷰가 저장할 SELECT문의 기반 테이블이 존재할 경우에만 생성(기본값)(선택)
4. `뷰이름` : (필수)
5. `열이름` : (생략가능)(선택)
6. `저장할 SELECT문` : (필수)
7. `WITH CHECK OPTION` : 지정한 제역 조건을 만족하는 데이터에 한해 DML 작업이 가능하도록 뷰 생성(선택)
8. `WITH READ ONLY` : 뷰의 열람, 즉 SELECT만 가능하도록 뷰 생성(선택)

```sql
-- 뷰 생성하기
CREATE VIEW VW_EMP20
    AS (SELECT EMPNO, ENAME, JOB, DEPTNO
          FROM EMP
          WHERE DEPTNO = 20);

-- 생성한 뷰 확인하기
SELECT *
  FROM USER_VIEWS;
```

### 뷰 삭제

```sql
DROP VIEW VW_EMP20;
```

삭제 후 `USER_VIEWS` 데이터 사전을 조회 해 보면 뷰가 삭제된 것을 알 수 있다. 뷰는 실제 데이터가 아닌 SELECT문만 저장하므로 뷰를 삭제해도 테이블이나 데이터가 삭제되는 것은 아니다.

### 인라인 뷰를 사용한 TOP-N SQL문

- 인라인 뷰 : SQL문에서 일회성으로 만들어서 사용하는 뷰

인라인뷰와 `ROWNUM`을 사용하면 `ORDER BY`절을 통해 정렬된 결과 중 최상위 몇 개 데이터만들 출력하는 것이 가능하다.

```sql
-- EMP 테이블을 SAL 열 기준으로 정렬하기
SELECT ROWNUM, E.*
  FROM EMP E;
ORDER BY SAL DESC;
```

`ROWNUM`은 `의사 열(pseudo column)`이라고 하는 특수 열이다. 의사 열은 데이터가 저장되는 실제 테이블에 존재하지는 않지만 특정 목정을 위해 테이블에 저장되어 있는 열처럼 사용 가능한 열을 뜻한다. `ROWID`도 대표적인 의사 열이다.

`ROWNUM`은 데이터를 추가할 때 매겨지는 번호이므로 `ORDER BY`
절로 정렬을 해도 기존에 매겨진 번호는 유지되는 특성이 있다.

이 특성을 인라인 뷰에서 적용하면 정렬된 SELECT문의 결과에 수번을 매겨서 출력할 수 있다.

```sql
--- 인라인 뷰로 TOP-N 추출하기
SELECT ROWNUM, E.*
  FROM (SELECT *
          FROM EMP
        ORDER BY SAL DESC) E -- 서브쿼리 부분을 E로 지정
  WHERE ROWNUM <= 3;
```

## 시퀀스 Sequence

오라클 데이터베이스에서 특정 규칙에 맞는 연속 숫자를 생성하는 객체

### 시퀀스 생성

```sql
CREATE SEQUENCE 시퀀스이름
[INCREMENT BY n]          -- 증가값(기본값:1)(선택)
[START WITH n]            -- 시작값(기본값:1)(선택)
[MAXVALUE n | NOMAXVALUE] -- 최대값 10^27
[MINVALUE n | NOMINVALUE] -- 최소값 -10^26
[CYCLE | NOCYCLE]         -- 최대값 도달 했을 경우 다시 시작 여부
[CACHE n | NOCACHE]       -- 메모리 할당해 놓은 수 지정, 기본값 20
```

```sql
CREATE SEQUENCE SEQ_DEPT_SEQUENCE
  INCREMENT BY 10
  START WITH 10
  MAXVALUE 90
  MINVALUE 0
  NOCYCLE
  CACHE 2;
```

### 시퀀스 사용

- `시퀀스이름.CURRVAL` : 시퀀스에서 마지막으로 생성된 번호를 반환함.
- `시퀀스이름.NEXTVAL` : 다음 번호를 생성한다.

```sql
-- 시퀀스에서 생성한 순번을 사용한 INSERT문 실행하기
INSERT INTO DEPT_SEQUENCE (DEPTNO, DNAME, LOC)
VALUES (SEQ_DEPT_SEQUENCE.NEXTVAL, 'DATABASE', 'SEOUL');
-- 위 INSERT문을 반복 사용하면 순번이 증가하면서 열이 생성된다.
```

### 시퀀스 수정

```sql
ALTER SEQUENCE 시퀀스이름
[INCREMENT BY n]
[MAXVALUE n | NOMAXVALUE]
[MINVALUE n | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE n | NOCACHE]
-- START WITH 값은 변경할 수 없다.
```

### 시퀀스 삭제

```sql
DROP SEQUENCE SEQ_DEPT_SEQUENCE;
```

시퀀스를 삭제해도 시퀀스를 사용하여 추가된 데이터는 삭제되지 않는다.

## 동의어 Synonym

테이블, 뷰, 시퀀스 등 객체 이름 대신 사용할 수 있는 다른 이름을 부여하는 객체이다. 별칭은 쿼리문 내부에서 지역변수처럼 사용되지만 동의어는 전역변수처럼 사용할 수 있다.

```sql
CREATE [PUBLIC] SYNONYM 동의어이름
   FOR [사용자.][객체이름];
```

- `PUBLIC` : 데이터베이스 내 모든 사용자가 사용할 수 있도록 설정하는 옵션. 생략할 경우 동의어를 생성한 사용자만 사용 가능(PUBLIC으로 생성되어도 본래 객체의 사용 권한이 있어야 사용 가능)(선택)

```sql
-- 동의어 생성 권한 부여 (SYSTEM 계정에서)
GRANT CREATE SYNONYM TO 사용자이름;
GRANT CREATE SYNONYM PUBLIC TO 사용자이름;
```

### 동의어 생성

```sql
CREATE SYNONYM E
   FOR EMP;
```

### 동의어 삭제

```sql
DROP SYNONYM E;
```
