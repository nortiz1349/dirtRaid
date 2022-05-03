---
title: "DB 제약 조건"
excerpt: Constraint, NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK, DEFAULT
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
date: 2022-05-03
---

## 제약 조건

제약 조건은 테이블에 저장할 데이터를 제약하는 특수한 규칙을 의미한다. 제약 조건을 설정한 열에는 조건에 맞지 않는 데이터를 저장할 수 없다. 이 제약 조건은 데이터의 정확성을 유지하기 위한 목적으로 사용하며 DDL로 설정할 수 있다.

### 데이터 무결성

`Data Integrity` 데이터베이스에 저장되는 데이터의 정확성과 일관성을 보장한다는 의미. 제약 조건은 이러한 데이터 무결성을 지키기 위한 안전장치로 중요한 역할을 한다.

- `영역 무결성(domain integrity)` : 열에 저장되는 값의 적정 여부를 확인. 자료형, 적절한 형식의 데이터, NULL 여부같은 정해 놓은 범위를 만족하는 데이터임을 규정.
- `개체 무결성(entity integrity)` : 테이블 데이터를 유일하게 식별할 수 있는 기본키는 반드시 값을 가지고 있어야 하며 NULL이 될 수 없고 중복될 수도 없을을 규정.
- `참조 무결성(referential integrity)` : 참조 테이블의 외래키 값은 참조 테이블의 기본키로서 존재해야 하며 NULL이 가능.

제약 조건은 데이터베이스 설계 시점, 즉 테이블을 생성할 때 주로 지정한다.

## 1. NOT NULL

특정 열에 데이터의 중복 여부와는 상관없이 NULL의 저장을 허용하지 않는 제약 조건. 반드시 열에 값이 존재해야 하는 경우에 지정한다.

### 제약 조건 지정

```sql
-- 테이블을 생성할 때 NOT NULL 설정하기
CREATE TABLE TABLE_NOTNULL(
        LOGIN_ID  VARCHAR2(20) [제약조건이름1] NOT NULL, -- 제약 조건에 이름을 지정할 수 있다.
        LOGIN_PWD VARCHAR2(20) [제약조건이름2] NOT NULL,
        TEL       VARCHAR2(20)
);
```

### 제약 조건 살펴보기

`USER_CONSTRAINTS` 데이터 사전을 활용한다.

```sql
SELECT OWNER, CONSTRAINT_NAME, CONSTRINT_TYPE, TABLE_NAME
  FROM USER_CONSTRAINTS;
```

- `CONSTRAINT_TYPE`  
  1) C: CHECK, NOT NULL
  2) U: UNIQUE
  3) P: PRIMARY KEY
  4) R: FOREIGN KEY

### 이미 생성한 테이블에 제약 조건 지정

```sql
-- TABLE_NOTNUL 테이블의 TEL 열에 NOT NULL 제약 조건 추가하기
ALTER TABLE TABLE_NOTNULL
  MODIFY(TEL [제약조건이름] NOT NULL);
```

제약 조건을 변경하려는 열에 이미 제약 조건과 맞지 않는 데이터가 있다면 제약 조건 지정이 불가하다.

### 제약 조건 이름 변경하기

```sql
ALTER TABLE [테이블이름]
RENAME CONSTRAINT [기존이름] TO [변경할이름];
```

### 제약 조건 삭제하기

```sql
ALTER TABLE [테이블이름]
DROP CONSTRAINT [제약조건이름];
```

## 2. UNIQUE

열에 저장할 데이터의 중복을 허용하지 않고자 할 때 사용한다. `NULL`은 중복 대상에서 제외되고 여러 개 존재 할 수 있다.

지정 방법은 `NOT NULL` 제약 조건과 동일하다.

## 3. PRIMARY KEY

`PRIMARY KEY` 제약 조건은 `UNIQUE`와 `NOT NULL` 제약 조건의 특성을 모두 가지는 제약 조건이다. 즉 중복을 허용하지 않고 `NULL`도 허용하지 않는다. 유일한 값을 가지므로 주민등록번호나 사원 번호같이 테이블의 각 행을 식별하는데 활용된다.

특정 열을 `PRIMARY KEY`로 지정하면 해당 열에는 자동으로 인덱스가 만들어진다.

## 4. FOREIGN KEY

외래키, 외부키로도 부르며 서로 다른 테이블 간 관게를 정의하는 데 사용하는 제약 조건이다. 특정 테이블에서 `PRIMARY KEY` 제약 조건을 지정한 열을 다른 테이블의 특정 열에서 참조하겠다는 의미로 사용할 수 있다.

부모 레코드, 자식 레코드 상속 관계로 생각하면 된다.

### FOREIGN KEY 지정하기

```sql
CREATE TABLE 테이블 이름(
  ...(다른 열 정의),
  열 자료형 CONSTRAINT [제약 조건 이름] FOREIGN KEY(열)
          REFERENCES 참조 테이블(참조할 열)
);
```

>1. EMP 테이블의 DEPTNO 열은 DEPT 테이블의 DEPTNO 열을 참조하여 저장값의 범위를 정한다.  
>2. 이렇게 되면 EMP-DEPTNO 에는 DEPT-DEPTNO에 존재하는 값과 NULL만 저장할 수 있게 된다.

### FOREIGN KEY로 참조 행 데이터 삭제하기

다른 테이블에서 참조가 되는 데이터인 부모 레코드는 삭제할 수 없다. 부모 레코드를 삭제하기 위해서는 자식 레코드를 먼저 삭제해야 하는 번거로운 일이 발생할 수 있다.

따라서 제약 조건을 처음 지정할 때 다음과 같이 추가 옵션을 지정하는 방법을 사용하기도 한다.

```sql
-- 열 데이터를 삭제할 때 이 데이터를 참조하고 있는 데이터도 함께 삭제
CONSTRAINT [제약조건이름] 
REFERENCES 참조테이블(참조열) ON DELETE CASCADE
```

```sql
-- 열 데이터를 삭제할 때 이 데이터를 참조하고 있는 데이터를 NULL로 수정
CONSTRAINT [제약조건이름] 
REFERENCES 참조테이블(참조열) ON DELETE SET NULL
```

`ALTER`문을 사용한 기능도 사용 가능하다.

## 5. CHECK

열에 저장할 수 있는 값의 범위 또는 패턴을 정의할 때 사용한다.

```sql
-- LOGIN_PWD 열 길이가 3 이상인 데이터만 저장 가능한 테이블 생성
CREATE TABLE TABLE_CHECK (
  LOGIN_ID  VARCHAR2(20) CONSTRAINT TBLCK_LOGINID_PK PRIMARY KEY,
  LOGIN_PWD VARCHAR2(20) CONSTRAINT TBLCK_LOGINPW_PK CHECK (LENGTH(LOGIN_PWD) > 3),
  TEL       VARCHAR2(20)
);
```

## DEFAULT

제약 조건과는 별개로 특정 열에 저장할 값이 지정되지 않았을 경우에 기본값을 지정할 수 있다.

```sql
-- LOGIN_PWD 값을 지정하지 않으면 기본값이 1234가 들어가는 테이블
CREATE TABLE TABLE_CHECK (
  LOGIN_ID  VARCHAR2(20) CONSTRAINT TBLCK_LOGINID_PK PRIMARY KEY,
  LOGIN_PWD VARCHAR2(20) DEFAULT '1234',
  TEL       VARCHAR2(20)
);
```
