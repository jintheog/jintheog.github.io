---
title: "React & JSX"
date: 2024-10-23 15:00:00 +0900
categories: [Front, React]
tags: [react, JSX]
---

# React

- React는 사용자 인터페이스를 구축하기 위한 JavaScript 라이브러리.
- 컴포넌트를 조립해서 UI를 만드는 라이브러리.

# JSX JavaScript XML

- xml : 데이터를 저장하고 전송하기 위한 구조를 정의 하는 언어
- JSX : JS 코드 안에서 html과 유사한 문법으로 UI구조를 작성할 수 있는 JS의 확장 문법; DOM API를 대체 한다.

```jsx
function App() {
  return (
    <div>
      <h1>Hello React!</h1>
    </div>
  );
}
```

# 1개의 최상위 태그만 반환 한다

- 여러 개의 태그를 반환할 수 없다. 반드시 1개의 부모 태그로 감싸야 한다
- Fragment 태그 `<> </>` 를 사용해서 여러 개의 태그를 감싸고, 반환한다

# 속성명은 대부분 camelCase로 작성한다
