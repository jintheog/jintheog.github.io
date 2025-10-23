---
title: "React Hooks 기초"
date: 2024-10-24 15:00:00 +0900
categories: [Front, React]
tags: [react, hooks, useState, useEffect]
---

## React Hooks

함수형 컴포넌트에서 상태 관리를 할 수 있게 해줍니다.

```jsx
import { useState, useEffect } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`카운트: ${count}`);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
