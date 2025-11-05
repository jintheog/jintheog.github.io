---
title: "Where, group by, aggregate function error "
date: 2025-10-23 12:00:00 +0900
categories: [Database, db-error]
tags: [sql, error, database]
---

# Where & Group By & 집계(aggregate)을 같이 쓸 경우

### where는 햄에 조건을 건다

### having은 그룹(집계 결과)에 조건을 건다

- COUNT(\*) 같은 **집계 함수**는 **WHERE에서 못쓰고 HAVING에서만** 쓸 수 있다.

#### 에러

```sql
-- 등급별 통계에서 영화가 100개 이상인 등급만 조회하세요:
-- - 등급, 영화 개수, 평균 대여료
select rating, count(*), avg(rental_rate)
from film
where count(*) > 100
group by rating

--Error Code: 1111. Invalid use of group function
```

1. WHERE: 그룹핑 전에 “행”을 거른다. (집계 함수 ❌)
2. GROUP BY: 행을 묶는다.
3. HAVING: 그룹핑 후 “그룹”을 거른다. (집계 함수 ✅)
