---
title: "Get method and refresh button Get 메소드 요청과 새로고침"
date: 2025-11-18 00:00:00 +0900
categories: [Spring, MVC]
tags: [Spring, MVC]
---

1. method를 적지 않으면 default로 get이다

# GET 요청은 새로고침을 눌러도 다시 전송된다.

- **form의 submit 버튼을 누르지 않아도,** 브라우저 새로고침만으로 GET 요청은 재전송된다.

# GET 요청의 특징: “안전(safe)하고 반복 가능(idempotent)한 요청”

- GET은 브라우저 기준에서 **“데이터를 가져오는 요청”**이기 때문에
  반복해서 보내도 문제없다고 전제되어 있음.

그래서:

- ✔ 주소창에 URL 입력 → GET 요청
- ✔ 링크 클릭 → GET 요청
- ✔ 폼(method="GET") 제출 → GET 요청
- ✔ 새로고침 클릭 → 방금 GET 요청을 다시 보냄

즉, **“저 URL로 페이지 로딩”**은 곧 GET 요청이고, 새로고침은 그 로딩을 다시 하는 거니까, GET 요청을 다시 보내는 것이 맞음.

---

### e.g.

`/search?keyword=apple`

- 이 URL을 띄워서 기다리는 상태에서 새로고침 누르면?

→ `/search?keyword=apple` GET 요청이 다시 서버로 감.

- 폼을 제출하지 않아도 동일함. 이미 URL 자체가 GET 요청을 포함하니까.

# ✅ 2. POST는 다르다

- POST는 새로고침 시 다음 팝업이 뜸:

> “폼 다시 제출할까요?”
>
> “Confirm form resubmission?”

- 왜냐하면 POST는 서버 상태를 바꾸는 요청일 수 있기 때문에 무한 반복하면 위험함.
  - 그래서 브라우저가 경고하는 것.

### GET 요청에서 새로고침 버튼을 누르면 URL 요청이 다시 전송되나? → YES

### form 안에 submit 버튼을 안 눌러도? → YES

- 페이지가 이미 GET 방식으로 로드된 상태라면 새로고침은 그 GET 요청을 반복하는 것일 뿐임.

### GET = 새로고침 시 요청 다시 보냄

### POST = 새로고침 시 브라우저가 경고
