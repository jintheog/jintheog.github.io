---
title: "FORM 태그"
date: 2024-10-22 15:00:00 +0900
categories: [HTML]
tags: [html, web, frontend]
---

# Form 태그

- 사용자로부터 정보를 입력받고 웹 서버로 전송하는 태그

### 폼 태그 역할

- 사용자에게 정보를 입력 받는다
- 입력된 정보를 웹 서버로 전송한다
- 여러 개의 입력 (input) 요소를 하나로 묶어 준다

#### 기본구조

```html
<form action="정보를 보낼 주소" method="보내는 방식">
  <!-- 입력 요소들 -->
</form>
```

- `action`: 입력된 정보를 보낼 주소 (URL)
- `method`: 정보를 보내는 방식

# 입력 input 태그

- 사용자가 정보를 입력할 수 있는 필드를 만든는 태그

#### 기본 구조

```html
<input type="입력 종류" name="입력_이름" value="기본값" />
```

- `type`: 입력 종류
- `name`: 입력 이름 (서버로 전송 할 떄 사용)
- `value`: 입력 필드에 미리 작성할 기본 값
- `placeholder`: 입력 필드의 안내 문구
- `required`: 필수 입력 필드로 설정

## 입력 태그 종류

### 글자 입력 태그

- `text`: 일반 글자 입력
- `password`: 비밀번호 입력
- `email`: 이메일 입력
- `number`: 숫자 입력
- `tel`: 전화번호 입력
- `url`: 주소 입력

```html
<!-- 일반 글자 입력-->
<input type="text" name="username" placeholder="사용자명을 입력하세요" />

<!-- 비밀번호 입력 (입력한 글자가 마스킹 처리됨) -->
<input type="password" name="password" placeholder="비밀번호를 입력하세요" />

<!-- 이메일 입력 (이메일 형식인지 자동으로 확인) -->
<input type="email" name="email" placeholder="이메일을 입력하세요" />

<!-- 숫자 입력 -->
<input type="number" name="age" min="1" max="100" />
```
