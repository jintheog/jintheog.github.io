---
title: "React component"
date: 2024-10-24 15:00:00 +0900
categories: [React]
tags: [react, component]
---

# 컴포넌트 (Component)

- 재사용 가능한 사용자 인터페이스 (UI) 블록으로 하나의 페이지는 여러 개의 컴포넌트를 조합 해서 만들어낸다.
- 컴포넌트는 독립적인 파일을 생성하고, 함수를 정의 해서 `요소(element)` 객체를 반환해야 한다
- `Default Export`로 컴포넌트를 내보내고, `import ... from...`로 컴포넌트를 가져와서 사용한다

# 컴포넌트 특징

- 재사용성: 변수처럼 여러 위치에서 반복하여 사용한다
- 독립성: 독립적으로 동작하며, 컴포넌트마다 별도의 데이터(상태)를 보유한다
- 계층 구조: 다른 컴포넌트를 포함할 수 있다

# 컴포넌트 작성 규칙

- 파일명과 컴포넌트명은 파스칼 케이스 PascalCase로 작성한다
- 하나의 파일에 하나의 컴포넌트만 정의한다.
- 변수처럼 역할과 기능을 표현하는 이름을 사용한다
- 열관된 확장자(.jsx)를 사용한다

# 컴포넌트 중첩 (Nested Component)

- 컴포넌트 내부에 또 다른 컴포넌트를 포함해서 사용하는 방식

### 컴포넌트 생성 및 내보내기

- `src/components` 폴더에 'jsx' 확장자 파일을 생성한다
- 파일명과 컴포넌트명은 파스칼 케이스 (PascalCase)로 작성한다
- 요소를 반환하는 함수를 작성하고 내보낸다

#### 기본구조

```jsx
function ComponentName() {
  return <div>컴포넌트 내용</div>;
}
export default ComponentName;
```
