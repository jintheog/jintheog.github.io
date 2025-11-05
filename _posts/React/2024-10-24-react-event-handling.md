---
title: "React Event Handling"
date: 2024-10-24 15:00:00 +0900
categories: [React]
tags: [react]
---

# Event Handling

- html 인라인 이벤트 속성 (`onClick`, `onChange` 등)을 사용해서 이벤트 핸들링 함수를 호출한다

- `addEventListener` 메서드는 사용하지 않는다
- `onInput` 이벤트는 사용하지 않는다

```jsx
export default function OnClick() {
  // 파라미터가 없는 함수
  function helloClick() {
    alert("Hello, World!");
  }

  // 파라미터가 있는 함수
  function handleClick(buttonName) {
    alert(`${buttonName} 클릭`);
  }

  return (
    <div>
      <div onClick={() => helloClick()}>Hello, World!</div>
      <button onClick={() => handleClick("1번 버튼")}>1번 버튼</button>
      <button onClick={() => handleClick("2번 버튼")}>2번 버튼</button>
      <button onClick={() => handleClick("3번 버튼")}>3번 버튼</button>
    </div>
  );
}
```
