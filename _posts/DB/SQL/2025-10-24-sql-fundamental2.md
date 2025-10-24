---
title: "Aggregate Functions"
date: 2025-10-24 10:00:00 +0900
categories: [Database, SQL]
tags: [sql, database]
---

# Aggregate Functions 집계 함수

- 여러 행의 데이터를 1개의 값으로 요약
- COUNT()
  - 행의 개수
  - COUNT(\*) → 전체 행 수
- SUM()
  - 합계
  - SUM(Population) → 총 인구
- AVG()
  - 평균
  - AVG(Population) → 평균 인구
- MAX()
  - 최댓값
  - MAX(Population) → 최대 인구
- MIN()
  - 최솟값
  - MIN(Population) → 최소 인구

### **집계 함수와 NULL**

### **NULL 처리 방식**

```sql
-- LifeExpectancy 통계
SELECT
    COUNT(*) AS 전체국가,
    COUNT(LifeExpectancy) AS 수명데이터있음,
    COUNT(*) - COUNT(LifeExpectancy) AS 수명NULL개수,
    ROUND(AVG(LifeExpectancy), 1) AS 평균수명
FROM country;

```

### **COALESCE로 NULL 처리**

```sql
-- NULL을 0으로 처리하여 평균 계산
SELECT
    AVG(LifeExpectancy) AS 평균1,  -- NULL 제외
    AVG(COALESCE(LifeExpectancy, 0)) AS 평균2  -- NULL을 0으로 처리
FROM country;
```
