---
title: "SubQuery"
date: 2025-10-27 12:00:00 +0900
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
