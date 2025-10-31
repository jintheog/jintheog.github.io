---
title: "React Props"
date: 2024-10-24 15:00:00 +0900
categories: [Front, React]
tags: [react, props]
---

# Props

- 부모 컴포넌트가 자식 컴포넌트에게 전달한 데이터를 저장한 객체
  - 부모 컴포넌트: Props를 전달한다
  - 자식 컴포넌트: Props를 받는다
- 동일한 컴포넌트를 재사용하기 위한 필수 문법

## 전달하기

- 자식 컴포넌트 요소 속성 (key=value) 형태로 Props를 전달한다

- `<컴포넌트명 PropsKey={PropsValue}.../>`

```jsx
import ChildComponent from "./components/ChildComponent";

export default function ParentComponent() {
  return (
    <div>
      <ChildComponent key={value} key2={value2} key3={value3} />
    </div>
  );
}
```

## 받기

- Props 객체는 컴포넌트 함수의 첫 번째 매개변수에 저장된다
  `function ChildComponent(props) {...}`

- `props.속성명` 형태로 전달받은 데이터에 접근 한다

### 기본 구조

```jsx
export default function ChildComponent(props) {
  return (
    <div>
      <li>{props.key}</li>
      <li>{props.key2}</li>
      <li>{props.key3}</li>
    </div>
  );
}
```
