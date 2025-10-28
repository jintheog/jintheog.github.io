---
title: "다대다 N:M & 재귀적 표현"
date: 2025-10-23 12:00:00 +0900
categories: [Database, fundamental]
tags: [erd, database]
---

# N:M

### 개념

양쪽 테이블 모두 여러 행과 연결된다.

### 문제점

RDBMS는 N:M 관계를 직접 구현할 수 없다.

### 해결 방법

**중간 테이블(연결 테이블, Junction Table)**을 생성하여 두 개의 1:N 관계로 분해합니다.

# e.g. Follow 재귀적 표현

- user가 user를 follow 할 경우
  - user 테이블이 user 테이블을 가리킴
  - 이걸 재귀적 표현 이라고도 함

Follow 테이블 :

- UniqueID
- from_user_id
- to_user_id

![Follow 테이블 다이어그램](/assets/img/DB/fundamental/2025-10-23-2025-10-23-db-fundamental.png)

- 사실상 follow 테이블이 중간 테이블이다.
  - user -> table -> user

### 역정규화

- 억지로 여러테이블을 하나의 테이블로 합치는 과정
