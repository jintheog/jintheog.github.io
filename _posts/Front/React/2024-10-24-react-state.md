---
title: "React State"
date: 2024-10-24 15:00:00 +0900
categories: [Front, React]
tags: [react, state]
---

# 상태 state

- 각 컴포넌트 내에서 독립적으로 관리되는 데이터
- 사용자 상호작용에 의해 변경될 수 있는 데이터
- 리액트는 상태가 변경되면 컴포넌트를 **리렌더링** (화면을 다시 그린다)
- 상태는 화면을 결정하는 중요한 데이터

상태는 컴포넌트마다 독립적으로 관리되는 데이터로 변수와는 다른 리액트의 데이터 관리 수단

변수와 상태의 차이점은 데이터가 변경될 때 리액트의 동작 유/무 입니다.

# userState Hook

- Hook 훅 : 리액트에서 특정한 동작을 수행하는 도구, 함수와는 다른 개념이다
- `상태를 저장하는변수`와 `상태 설정함수`를 생성하는 상태 관리 도구

### 기본 구조

```jsx
import { useState } from "react";
function ComponentName() {
  const [something, setSomething] = useState(initialValue);
}
```

### 문자열 상태 업데이트

```jsx
import { useState } from "react";

export default function StringState() {
  // 초기 값이 문자열 ""인 상태를 생성한다
  const [stringState, setStringState] = useState("");

  function updateStringState() {
    // 상태는 직접 수정할 수 없다
    // ❎ stringState = "추가"

    // 새로운 문자열을 생성해서 상태를 변경
    let newString = `${stringState} 추가`;
    setStringState(newString);
  }

  return (
    <div>
      <p>{stringState}</p>
      <button
        onClick={() => {
          updateStringState();
        }}
      >
        문자열 추가
      </button>
    </div>
  );
}
```

## 숫자 상태 업데이트

```jsx
import { useState } from "react";

export default function NumberState() {
  // 초기 값이 숫자 0인 상태를 생성한다
  const [numberState, setNumberState] = useState(0);

  function updateNumberState() {
    // 상태는 직접 수정할 수 없다
    // ❎ numberState = numberState + 1;

    // 새로운 숫자형 데이터를 생성해서 상태를 변경
    let newNumber = numberState + 1;
    setNumberState(newNumber);
  }

  return (
    <div>
      <p>{numberState}</p>
      <button
        onClick={() => {
          updateNumberState();
        }}
      >
        카운트 증가
      </button>
    </div>
  );
}
```

### 객체 상태 수정

```jsx
import { useState } from "react";

export default function ObjectState() {
  const [objectState, setObjectState] = useState({
    age: 19,
    name: "홍길동",
  });

  function updateObjectState() {
    // ... 연산자로 새로운 객체를 생성해서 변경
    // 수정할 속성만 새로운 값으로 변경
    let newObjectState = {
      ...objectState,
      age: objectState.age + 1,
    };

    setObjectState(newObjectState);
  }

  return (
    <div>
      <p>이름 : {objectState.name}</p>
      <p>나이 : {objectState.age}</p>
      <button
        onClick={() => {
          updateObjectState();
        }}
      >
        나이 증가
      </button>
    </div>
  );
}
```
