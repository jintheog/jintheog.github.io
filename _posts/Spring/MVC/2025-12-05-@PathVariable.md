---
title: "@PathVariable"
date: 2025-11-18 00:00:00 +0900
categories: [Spring, MVC]
tags: [Spring, MVC, annotation]
---

# ✔️ PathVariable 은 URL 자체에서 값을 읽어온다다.

즉,

> <form> 태그에서 hidden input 같은 것을 보내주는 게 아니라, form의 action URL 자체에 id가 포함되어 있으므로 Spring MVC가 그 값을 자동으로 PathVariable로 가져오는 것.

### ✔️ 실제 동작 과정 (완벽하게 설명)

#### 1. form action 생성

form.html :

```html
th:action="${isEdit} ? @{/posts/{id}(id=${postId})} : @{/posts}"
```

만약 postId = 5 라고 하면,

Thymeleaf가 이것을 해석해서 action을 다음과 같이 만들어줌:

```bash
POST /posts/5
```

또는

```bash
POST /posts/5/edit
```

(지금 너는 @PostMapping("/{id}/edit") 형태이므로 실제 action 결과는 /posts/5/edit)

그러니까 form 제출 시 요청 URL이 이미 id 값을 포함하고 있음.

#### 2. 브라우저는 이 URL로 POST 요청을 보냄

```bash
   POST http://localhost:8080/posts/5/edit
```

#### 3. Spring MVC는 URL 패턴 매칭을 함

컨트롤러의 매핑:

```java
@PostMapping("/{id}/edit")
public String update(@PathVariable Long id, ...)
```

Spring은:

- `{id}` 자리에 있는 값을 URL에서 추출

- `5`를 Long으로 변환

- 그 값을 update()의 `id` 파라미터로 주입

즉,

🔥 PathVariable 은 "URL 자체"에서 값을 가져오므로

form에서 값을 넘겨줄 필요가 없다.

---

### ✔️ 아주 정확한 결론

#### 👉 폼에서 PathVariable 로 전달되는 것이 아니라,

폼의 action URL 에 포함된 {id} 값을 Spring 이 PathVariable 로 읽는 것.
