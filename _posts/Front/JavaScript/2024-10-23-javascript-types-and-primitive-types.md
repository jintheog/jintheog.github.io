---
title: "JavaScript 자료형 & 원시 자료형"
date: 2024-10-23 14:00:00 +0900
categories: [Front, JavaScript]
tags: [javascript, web, programming]
---

# 자료형 data types

### 자료형

- 데이터의 종류 (e.g. 숫자, 문자, 참/거짓)
- 데이터 종류에 따라 수행 가능한 작업이 달라짐

### 원시 자료형

- 가장 기본적인 데이터 타입
- 불변성 - 생성 후 값 변경 불가

자료형 확인

- `typeof 데이터`: 데이터의 자료형 확인

## 기본 자료형

### 문자열 (string)

- 글자의 나열
- 3개의 표현 방법
  - 작은 따옴표 (''), 큰 따옴표(""), 백틱(`)

### 숫자형 Number

- 모든 종류의 숫자
- 정수와 실수 포함

### 불리언 (boolean)

- 참(true) 또는 거짓 (false)
- "맞다"/"틀리다" 표현에 사용

## 특별한 자료형

### undefined

- "정해진 값이 없다"
- 변수를 만들었지만 아직 값을 넣지 않았을 때 자동으로 생긴다

### null

- "빈 값"
- 개발자가 "여기에는 아무것도 없다"라고 일부러 표시할때 사용

| 구분         | null                 | undefined                 |
| ------------ | -------------------- | ------------------------- |
| 의미         | "일부러 비워둠"      | "자동으로 비워짐"         |
| 누가 만드나? | 개발자가 직접 넣는다 | 컴퓨터가 자동으로 만든다0 |
| typeof 결과  | object               | undefined                 |
| 사용 예시    | `let data = null;`   | `let data; //undefined`   |

![alt text](/assets/img/js/2024-10-23-javascript-types-and-primitive-types.png)
