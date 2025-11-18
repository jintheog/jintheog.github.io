---
title: "Path always begins with slash"
date: 2025-11-18 00:00:00 +0900
categories: [Spring, MVC]
tags: [Spring, MVC]
---

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <title>new</title>
  </head>
  <body>
    <h1>NEW</h1>
    <form action="/todos/create">
      <input type="text" name="title" />
      <input type="text" name="content" />
      <input type="submit" />
    </form>
  </body>
</html>
```

# 모든 경로에는 `/` 슬래시가 붙어야 함

- 최상위 도메인으로 시작 하겠다는 뜻.
- `/`슬래시가 없으면 지금 도메인에서 시작하겠다
