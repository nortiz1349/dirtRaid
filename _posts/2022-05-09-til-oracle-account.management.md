---
title: "DB 사용자 관리"
excerpt: user, privilege, roll
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
date: 2022-05-09
---

## 데이터베이스 스키마란?

데이터베이스에서 데이터 간 관계, 데이터 구조, 제약 조건 등 데이터를 저장 및 관리하기 위해 정의한 데이터베이스 구조의 범위를 스키마(schema)를 통해 그룹 단위로 분류한다.

오라클에서는 스키마와 사용자를 구별하지 않고 사용하기도 한다. 사용자는 데이터베이스에 접속하는 개체를 뜻하고, 스키마는 접속한 사용자와 연결된 객체를 의미한다.

## 사용자 생성, 변경, 삭제

```sql
CREATE USER 사용자이름(필수)
IDENTIFIED BY 패스워드(필수)
DEFAULT TABLESPACE (선택)
TEPORARY TABLESPACE (선택)
QUOTA 테이블 스페이스크기 ON (선택)
PROFILE (선택)
PASSWORD EXPIRE (선택)
ACOUNT [LOCK/UNLOCK]; (선택)
```

```sql
-- 데이터베이스 관리 권한을 가진 사용자가 생성 가능하다.
CREATE USER ORCLSTUDY
IDENTIFIED BY ORACLE;
```

새로 생성한 사용자는 데이터베이스 연결을 위한 권한, 즉 CREATE SESSION 권한을 부여 받지 못했기 때문에 접속이 불가능하다. `GRANT`문으로 접속 권한을 부여한다.

```sql
GRANT CREATE SESSION TO ORCLSTUDY;
```

사용자 또는 사용자 소유 객체 정보를 얻기 위해 다음 데이터 사전을 사용할 수 있다.

```sql
SELECT * FROM ALL_USERS
  WHERE USERNAME = 'ORCLSTUDY';
```

```sql
SELECT * FROM DBA_USERS
  WHERE USERNAME = 'ORCLSTUDY';
```

```sql
SELECT * FROM DBA_OBJECTS
  WHERE OWNER = 'ORCLSTUDY';
```

- `ALTER USER` 와 `DROP USER`문으로 사용자 정보 변경 및 삭제가 가능하다.

## 권한 관리

### 시스템 권한 System Privilege

오라클 데이터베이스의 시스템 권한은 사용자 생성과 정보 수정 및 삭제, 데이터베이스 접근, 오라클 데이터베이스의 여러 자원과 객체 생성 및 관리 등의 권한을 포함한다. 이러한 내용은 데이터베이스 관리 권한이 있는 사용자가 부여할 수 있는 권한이다.

- 시스템 권한 부여

  ```sql
  GRANT [시스템 권한] TO [사용자이름/롤이름/PUBLIC]
  [WITH ADMIN OPTION];
  ```

  - 시스템 권한(필수) : 오라클에서 제공하는 시스템 권한을 지정한다. 쉼표로 구분하여 여러 권한을 부여할 수 있다.  
  - 사용자이름/롤이름/PUBLIC(필수) : 권한을 부여하는 대상을 지정한다.  
  - WITH ADMIN OPTION : 현재 GRANT문을 통해 부여받은 권한을 다른 사용자에게도 부여할 수 있는 권한도 함께 부여받는다. 현재 사용자의 권한이 사라져도, 권한을 재부여한 다른 사용자의 권한은 유지된다.

- 시스템 권한 취소

  ```sql
  REVOKE [시스템권한] FROM [사용자이름/롤이름/PUBLIC];
  ```

### 객체 권한 Object Privilege

객체 권한이란 특정 사용자가 생성한 테이블, 인덱스, 뷰, 시퀀스 등과 관련된 권한이다.

<https://docs.oracle.com/cd/B28359_01/server.111/b28286/statements_9013.htm#BGBCIIEG>

- 객체 권한 부여

  ```sql
  GRANT [객체권한/ALL PRIVILIEGES]
    ON  [스키마.객체이름]
    TO  [사용자이름/롤/PUBLIC]
  [WITH GRANT OPTION]
  ```
  
- 객체 권한 부여

  ```sql
  REVOKE [객체권한/ALL PRIVILIEGES]
    ON  [스키마.객체이름]
    FROM  [사용자이름/롤/PUBLIC]
  [CASCADE CONSTRAINTS/FORCE]
  ```

## 롤 관리

사용자를 신규로 생성할때마다 권한을 일일이 부여하는 것은 너무나 번거로운 일이다. 이러한 불편함을 해결하기 위해 롤(role)을 사용합니다. 롤은 여러 권한을 묶어 놓은 그룹을 뜻한다.

롤은 기본적으로 제공되는 `predefined roles`과 `user roles`로 나누어집니다.

### Predefined Roll

- CONNECT ROLL

  ```sql
  ALTER SESSION, 
  CREATE CLUSTER, 
  CREATE DATABSE LINK,
  CREATE SEQUENCE, 
  CREATE SESSION, 
  CREATE SYNONYM,
  CREATE TABLE, 
  CREATE VIEW
  ```

- RESOURCE ROLL

  ```sql
  CREATE TRIGGER,
  CREATE SEQUENCE,
  CREATE TYPE,
  CREATE PROCEDURE,
  CREATE CLUSTER,
  CREATE OPRATOR,
  CREATE INDEXTYPE,
  CREATE TABLE
  ```

- DBA ROLL : 시스템 권한 대부분을 가짐.

### USER ROLL

