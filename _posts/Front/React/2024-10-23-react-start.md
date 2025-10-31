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

# JSX 보간법 interpolation

- 중괄호 `{}`를 사용하여 JSX 내부에 표현식을 삽입 하는 방법
- 문자열 사이에 표현식을 삽입하는 **템플릿 리터럴**과는 다른 개념

변수 삽입

```jsx
const name = "김철수";
const age = 25;

const greeting = <h1>안녕하세요, {name}님!</h1>;
const profile = <p>나이: {age}세</p>;
```

표현식 삽입

```jsx
const a = 10;
const b = 20;

const calculation = <p>계산 결과: {a + b}</p>;
const comparions = <p>더 큰수: {a > b ? a : b}</p>;
const isAdult = <p>성인 여부: {age >= 18 ? "성인" : "미성년자"}</p>;
```

함수 호출 삽입

```jsx
function getCurrenTime() {
  return new Date().toLocaleTimeString();
}

const timeDisplay = <p>현재 시간: {getCurrentTime()}</p>;
const upperCaseName = <h2>{name.toUpperCase()}</h2>;
```

객체 속성 삽입

```jsx
const user = {
  name: "이영희",
  email: "younghee@example.com"
  age: 28,
};

const userInfo = (
  <>
    <h1>{user.name}</h1>
    <p>이메일: {user.email}</p>
    <p>나이: {user.age}세</p>
  <>
);
```

배열 요소 삽입

```jsx
const colors = ["빨강", "파랑", "초록"];
const numbers = [1, 2, 3, 4, 5];

const colorList = (
  <>
    <p>첫 번째 색상: {colors[0]}</p>
    <p>두 번째 색상: {colors[1]}</p>
    <p>숫자 합계: {numbers[0] + numbers[1] + numbers[2]</p>
  <>
);
```

# 주의사항

- 조건문, 반복문, 객체 데이터는 사용할 수 없다
- 객체는 꼭 속성 데이터를 사용한다

```jsx
// 조건문은 사용 불가
{
  /* {if (true) { return "참"; }} */
}

// 삼항 연산자 표현식은 사용 가능
{
  true ? "참" : "거짓";
}

// for문은 사용 불가
{
  /* {for (let i = 0; i < 5; i++) { console.log(i); }} */
}

// 배열 고차 메서드는 사용 가능
{
  [1, 2, 3].map((number) => number * 2);
}

// 객체 원본은 사용 불가
{
  /* {user} */
}
// 객체의 속성은 사용 가능
{
  user["name"];
}
```
