---
title: "CSS 선택자"
date: 2024-10-23 13:00:00 +0900
categories: [Front, CSS]
tags: [css, selector]
---

# 선택자

- 스타일을 적용할 HTML 요소를 선택하는 방법
- 태그(tag), 클래스(class), 아이디(id) 등으로 요소를 선택한다

## 기본 선택자

### 전체 선택자 universal selector

문서 내 모든 요소 선택

```css
* {
  속성: 값;
}
```

### 태그 선택자 type selector

- 특정 태그의 모든 요소를 선택
- `태그명` 형태로 작성

```css
태그명 {
  속성: 값;
}

/* 모든 p 태그의 글자를 파란색으로 만들기 */
p {
  color: blue;
}

/* 모든 h1 태그의 글자를 크고 굵게 만들기 */
h1 {
  font-size: 32px;
  font-weight: bold;
}
```

### 클래스 선택자 class selector

- 특정 클래스의 모든 요소를 선택
- 여러 요소에 같은 클래스를 사용할 수 있다
- 하나의 요소에 여러 클래스를 사용할 수 있다
- 재사용성이 높다
- `.클래스명` 형태로 작성

```css
.클래스명 {
  속성: 값;
}

.highlight {
  /* 클래스가 highlight인 요소의 배경을 노란색으로 만들기 */
  background-color: yellow;
  font-weight: bold;
}
```

### 아이티 선택자 id selector

- 고유한 id 속성을 가진 요소 하나를 선택
- 문서에서 유일한 요소를 선택할 때 사용한다
- `#아이디명` 형태로 작성한다

```css
#아이디명 {
  속성: 값;
}

#main-header {
  background-color: navy;
  color: white;
  padding: 20px;
}
```
