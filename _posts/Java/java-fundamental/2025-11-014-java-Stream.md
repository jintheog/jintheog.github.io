---
title: "Stream API"
date: 2025-11-14 00:00:00 +0900
categories: [Java, java-fundamental]
tags: [OOP, Java]
---

# 스트림 API의 각 메서드가 이미 적절한 함수형 인터페이스 타입을 파라미터로 받고 있어서, 람다식만 넣으면 자동으로 그 타입에 맞춰 적용되는 것

- 우리가 직접 타입명(Consumer, Function, BiPredicate…)을 명시할 필요가 없도록 Stream이 설계되어 있음

### Stream 메서드는 내부에서 어떤 인터페이스를 받는가

| Stream 메서드 | 내부에서 요구하는 함수형 인터페이스          |
| ------------- | -------------------------------------------- |
| `.filter()`   | `Predicate<T>`                               |
| `.map()`      | `Function<T, R>`                             |
| `.forEach()`  | `Consumer<T>`                                |
| `.sorted()`   | `Comparator<T>`                              |
| `.reduce()`   | `BinaryOperator<T>` 또는 `BiFunction<T,T,T>` |
| `.collect()`  | `Collector`                                  |

### e.g.

` .filter(item → item % 2 == 0)`

- 이렇게만 적어도 자바 컴파일러는 자동으로 이렇게 해석 하다 :

```java
Predicate<Integer> predicate = item -> item % 2 == 0;
```

# 실제 내부 동작 (자바가 하는 일)

- 적는 코드 :

```java
numbers.stream()
       .filter(item -> item % 2 == 0)
```

- 자바가 실제로 해석하는 방식 :

```java
Predicate<Integer> tmp = new Predicate<>() {
    @Override
    public boolean test(Integer item) {
        return item % 2 == 0;
    }
};

numbers.stream().filter(tmp);

```

- **람다 → 익명 클래스 → Predicate 형태**로 자동 변환됨.
