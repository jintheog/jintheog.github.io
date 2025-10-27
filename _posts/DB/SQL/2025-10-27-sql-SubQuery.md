---
title: "SubQuery"
date: 2025-10-27 02:00:00 +0900
categories: [Database, SQL]
tags: [sql, database]
---

```sql
-- 'Action' 카테고리에 속한 영화를 조회하세요.
-- - 영화 제목
select category_id from category where name = 'Action'
;

select film_id from film_category where category_id = (select category_id from category where name = 'Action')
;

select title
from film
where film_id in(select film_id from film_category where category_id = (select category_id from category where name = 'Action'))
;

```

1. `select category_id from category where name = 'Action'`

   - 카테고리 테이블에서 Action 카테고리의 pk category_id를 조회

2. `select film_id from film_category where category_id = (select category_id from category where name = 'Action');`
   - film_category 테이블에서 그 Action 이라는 카테고리의 category_id에 해당하는 영화들의 pk film_id를 모두 조회
3. 그 film_id들을 가진 영화제목을 film 테이블에서 모두 조회

## JOIN과 같이 써서 해결

```sql
select
f.title,
c.name as category
from film f left join film_category fc on f.film_id = fc.film_id
join category c on c.category_id = fc.category_id
where fc.category_id = (select category_id from category where name = 'Action');

```

# 예제

```sql

-- 고객별 대여 횟수를 구한 뒤, 대여 횟수가 30회 이상인 고객만 조회하세요.
-- - 고객 이름, 대여 횟수, 대여 횟수 내림차순
SELECT
	c_name, c_id, c_count
FROM
(
	SELECT
		c.last_name AS c_name,
		c.customer_id AS c_id,
		COUNT(*) AS c_count
	FROM customer c INNER JOIN rental r
	ON c.customer_id = r.customer_id
	GROUP BY c.customer_id
) AS customer_rental
WHERE c_count >= 30;
```

```sql
-- 대여 기록이 있는 고객만 조회하세요.
-- - 고객 이름 (first_name, last_name), 이메일
select
first_name,
last_name,
email
from customer
where customer_id IN (select customer_id from rental);

select
first_name, last_name, email
from customer
where
	exists (select * from rental where customer.customer_id)
;
```
