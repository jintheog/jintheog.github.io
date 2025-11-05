---
title: "Subquery IN vs = ; error code 1242"
date: 2025-10-27 12:00:00 +0900
categories: [Database, db-error]
tags: [sql, error, database]
---

## Error Code: 1242. Subquery returns more than 1 row

### 에러 나는 쿼리

```sql
select title from film where film_id = (select film_id from film_category where category_id = (select category_id from category where name = 'Action'))
```

- 이 쿼리에서 WHERE film_id = (subquery) 는 `=` 연산자를 사용하고 있다.
  > film_id 는 하나의 값과만 비교할 수 있다는 뜻. 그런데 서브쿼리가 여러 film_id를 반환하기 때문에 오류

### 수정

```sql
select title
from film
where film_id in(select film_id from film_category where category_id = (select category_id from category where name = 'Action'))
;
```

### IN 사용 예

```sql
select first_name, last_name, email from customer where customer_id IN (select customer_id from rental);
```

✅ 차이 요약

= (equal) : ✅ 단일 값 ❌ 여러 값
IN (...) : ❌ 단일 값 ✅ 여러 값
