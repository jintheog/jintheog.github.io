---
title: "Alias-Parenthesis Error"
date: 2025-10-23 12:00:00 +0900
categories: [Database, db-error]
tags: [sql, error, database]
---

# alias

- alias 에 () 괄호 사용 불가

```sql
select
min(fare),
max(fare),
round(avg(fare),2) as avg(fare)
from titanic;

```

Error Code: 1064. You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '(fare) from titanic' at line 4 0.000 sec
