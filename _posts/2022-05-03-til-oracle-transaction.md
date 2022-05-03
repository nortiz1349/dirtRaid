---
title: "DB 트랜잭션 제어와 세션"
excerpt: TRANSACTION, COMMIT, ROLLBACK, LOCK
excerpt_separator: "<!--more-->"
categories:
  - DATABASE
tags:
  - oracle
  - sql
  - Do it!
  - transaction
  - TIL

toc: true
toc_sticky: true
date: 2022-05-03
---

## 트랜잭션이란

`TRANSACTION` 더 이상 분할할 수 없는 최소 수행 단위. 계좌 이체와 같이 하나의 작업 또는 밀접하게 연관된 작업을 수행하기 위해 한 개 이상의 데이터 조작 명령어`(DML:Data Manipulation Language)`로 이루어진다.

트랜잭션은 하나의 트랜잭션 내에 있는 여러 명령어를 한 번에 수행하여 작업을 완료하거나 아예 모두 수행하지 않는 상태, 즉 모든 작업을 취소한다. 이러한 특성을 `all or nothing` 이라고도 하며, 트랜잭션을 제어하기 위해 사용하는 명령어를 `TCL`(Transaction Control Language)라고 한다.

트랜잭션은 데이터베이스 계정을 통해 접속하는 동시에 시작된다. 접속 후 여러 SQL문을 실행하고 트랜잭션을 제어하는 명령(TCL)을 실행할 때 기존 트랜잭션이 끝나게 된다. `DDL`(Data Definition Language), `DCL`(Data Control Language) 명령어를 샤용할 때도 트랜잭션의 종료, 시작되는 효과가 있다.

## 트랜잭션을 제어하는 명령어

### ROLLBACK

트랜젝션을 취소할 때 사용한다. `ROLLBACK`을 통해 작업을 취소할 지점을 정할때 `SAVEPOINT` 명령어를 사용할 수 있다.

### COMMIT

트랜잭션을 데이터베이스에 영구히 반영할 때 사용한다. 트랜잭션 작업이 정상적으로 수행되었다고 확신할 때 사용해야 한다. `COMMIT`을 통해 트랜잭션이 종료되고 되돌릴 수 없다.

## 세션과 읽기 일관성의 의미

### 세션

`SESSION` 오라클 데이터베이스에서 세션은 데이터베이스 접속을 시작으로 여러 데이터베이스에서 관련 작업을 수행한 후 접속을 종료하기까지 전체 기간을 의미한다.

### 읽기 일관성

`READ CONSISTANCY` 데이터베이스는 여러 곳에서 동시에 접근하여 데이터를 관리, 사용하는 것이 목적이므로 대부분 수많은 세션이 동시에 연결되어 있다. 읽기 일관성이란 특정 세션에서 테이블의 데이터를 변경 중일 때, 데이터를 변경 중인 세션을 제외한 나머지 세션은 변경과 무관한 본래의 데이터를 보여주는 특성을 의미한다.

## 수정 중인 데이터 접근을 막는 LOCK

### LOCK

특정 세션(세션A)에서 조작중인 데이터는 트랜잭션이 완료되기 전까지 다른 세션(세션B)에서 조작할 수 없는 상태가 된다. 이 상태를 `LOCK`이라고 한다.

만약 `세션A`에서 데이터를 조작중인 상태에서 `세션B`가 데이터를 조작한다면, 세션B의 데이터 조작은 세션A 데이터 조작이 완료되는 것을 (COMMIT이나 ROLLBACK 하기 전, LOCK이 풀리는 것을) 기다리게 되는데, 이 현상을 `HANG`이라고 한다. 세션B의 `HANG` 은 세션A의 `LOCK`이 풀리는 즉시 실행한다.

### row level lock

SQL문으로 조작하는 대상 데이터가 테이블의 특정 행 데이터일 경우 해당 행만 LOCK이 발생한다.

### table level lock

WHERE절을 사용하지 않고 테이블의 모든 데이터에 영향을 주는 경우를 말한다. 다만 INSERT문은 수행 가능하며, 이미 저장된 데이터에 대한 UPDATE, DELETE문 사용시 HANG 상태로 대기하게 된다.
