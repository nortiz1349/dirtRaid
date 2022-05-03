---
title: "DB 데이터 조작어"
excerpt: INSERT, UPDATE, DELETE
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

- `DDL(DATA DEFINITION LANGUAGE)` : 데이터 정의어  
CREATE, ALTHER, DROP, RENAME, TRUNCATE
- `DCL(DATA CONTROL LANGUAGE)` : 데이터 제어어  
GRANT, REVOKE
- `DML(DATA MANIPULATION LANGUAGE)` : 데이터 조작어  
SELECT, INSERT, UPDATE, DELETE

## 테이블에 데이터 추가하기

### 1) 테이블 생성

```sql
CREATE TABLE DEPT_TEMP
  AS SELECT * FROM DEPT;
-- 테이블 복제 : 칼럼 안에 설정된 기본키 같은 것은 복제가 되지 않는다.
```

### 2) 테이블 삭제

```sql
DROP TABLE 테이블이름;
```

### 3) 데이터 추가

```sql
INSERT INTO 테이블이름 [(열1, 열2, ..., 열N) 생략가능] -- 생략시에는 테이블에 칼럼 순서를 지켜야 한다.
VALUES (열1데이터, 열2테이터, ... , 열N데이터);

INSERT INTO DEPT_TEMP (DEPNO, DNAME, LOC)
              VALUES (50, 'DATABASE', 'SEOUL');
```

- INSERT문에서 지정한 열 개수와 입력할 데이터 개수가 일치하지 않거나 자료형이 맞지 않은 경우, 열 길이를 초과하는 경우는 오류가 발생한다.
- 열 지정을 생략할 경우 테이블을 만들 때 설정한 열 순서대로 나열되어 있다고 가정하고 데이터를 작성해야 한다.

### 4) NULL 데이터 입력

- 명시적 입력 : `NULL`을 직접 입력, `''` 빈공백 문자열로 입력.
- 암시적 입력 : 아예 입력하지 않음.

### 5) 날짜 데이터 입력

```sql
CREATE TABLE EMP_TEMP
  AS SELECT *
    FROM EMP
    WHERE 1 <> 1; -- 거짓:참(1)이 아닌경우 실행 - 데이터는 가져오지 않고 칼럼명과 칼럼 형식만 가져온다.
```

- 기본형식

`YYYY/MM/DD`, `YYYY-MM-DD` 형식으로 날짜데이터 입력

```sql
-- 기본형식
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
            VALUES (1111, '성춘향', 'MANAGER', 9999, '2001-01-05', 4000, NULL, 20);
```

- `T_DATE` 형식

```sql
-- TO_DATE
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
            VALUES (2111, '이순신', 'MANAGER', 9999, TO_DATE('07/01/2001', 'DD/MM/YYYY'), 4000, NULL, 20);
```

- `SYSDATE` 형식  
현재 시점으로 날짜를 입력할 경우

### 6) 서브쿼리 활용

서브쿼리를 사용하여 한 번에 여러 데이터 추가가 가능하다.

```sql
-- EMP 테이블에서 월급등급이 1인 사원만 EMP_TEMP에 넣기
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
    SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIRERATE, E.SAL, E.COMM, E.DEPTNO
      FROM EMP E, SALGRADE S
     WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
       AND S.GRADE = 1;
```

- VALUE절은 사용하지 않는다.
- 데이터가 추가되는 테이블의 열 개수와 서브쿼리의 열 개수가 일치해야 한다.
- 데이터가 추가되는 테이블의 자료형과 서브쿼리의 자료형이 일치해야 한다.

## 테이블에 있는 데이터 수정하기

### 1) `UPDATE` 기본 사용법

```sql
UPDATE [변경할 테이블]
SET [변경할 열1]=[데이터], ... [변경할 열N]=[데이터]
WHERE [변경할 대상 행을 선별하기 위한 조건];
-- WHERE 조건문 사용 권장
```

### 2) 데이터 전체 수정

```sql
UPDATE DEPT_TEMP2
  SET LOC = 'SEOUL';
-- WHERE 조건문을 사용하지 않음
```

### 3) 수정한 내용 되돌리기

`ROLLBACK;` : Transaction 명령어 중에 하나. 직전 실행된 명령어를 되돌림.

### 4) 데이터 일부분만 수정하기

```sql
-- WHERE 조건문을 사용
UPDATE  DEPT_TEMP2
   SET  DNAME = 'DATABASE',
        LOC   = 'SEOUL'
 WHERE  DEPTNO = 40;
```

### 5) 서브쿼리 활용

```sql
UPDATE DEPT_TEMP2
   SET (DNAME, LOC) = (SELECT DNAME, LOC
                        FROM  DEPT
                        WHERE DEPTNO = 40)
  WHERE DEPTNO = 40;
```

## 테이블에 있는 데이터 삭제하기

```sql
DELETE FROM [테이블이름]
WHERE 선별식;

DELETE FROM EMP_TEMP2
  WHERE JOB = 'MANAGER';
-- JOB열 데이터가 MANAGER인 데이터만 삭제
```

- `DELETE`, `UPDATE`문은 사용할 때 특별히 주의해야 한다. `WHERE` 조건식을 사용하여 삭제할 대상을 정확히 선택하고 있는지 검증하는 과정이 필요하다. `SELECT`문을 사용하여 반드시 검증한 후 사용한다.
