---
title: "Varargs 가변인자"
date: 2025-11-03 00:00:00 +0900
categories: [Java, java-fundamental]
tags: [OOP, Java]
---

```java
public class VarargsRules {
    // OK - 가변 인자는 마지막에
    void method1(String name, int... scores) { }

    // 컴파일 에러! 가변 인자 뒤에 다른 매개변수 불가
    // void method2(int... scores, String name) { }

    // 컴파일 에러! 가변 인자는 하나만
    // void method3(int... a, String... b) { }

    // OK - 배열로 전달 가능
    void method4(int[] scores) { }
}

```

# 가변 인자가 배열로 처리되기 때문

### 가변 인자(Varargs)란?

```java
void method1(int... scores) { }
```

- 이건 실제로 컴파일되면 내부적으로

```java
void method1(int[] scores) { }
```

로 바뀜. 즉, 가변 인자는 사실상 **배열 매개변수**와 같다.

### 호출 시:

```java
method1(10, 20, 30);
```

이건 결국 컴파일러가 알아서

```java
method1(new int[]{10, 20, 30});
```

로 바꿔주는 것.

### 그런데 문제는 가변 인자 뒤에 다른 매개변수를 두면 모호해진다

예를 들어 아래처럼 쓰면:

```java
void method2(int... scores, String name) { } // ❌ 컴파일 에러
```

이걸 호출할 때,

```java
method2(10, 20, 30, "Kim");
```

자바 컴파일러는 헷갈린다

- "Kim"을 scores 배열의 일부로 넣어야 하나?

- 아니면 "Kim"이 뒤의 name으로 가야 하나?

경계가 모호하기 때문에 자바는 아예 이런 형태를 금지시킨 것

> 그래서 규칙이 생김:

`가변 인자(…) 는 반드시 매개변수 목록의 마지막에만 올 수 있다.`

### 올바른 형태

```java
void method1(String name, int... scores) { }
```

호출 시:

```java
method1("Kim", 90, 80, 70);
```

- → 첫 번째 인자는 name
- → 나머지(90,80,70)는 전부 scores 배열에 자동으로 묶임.
