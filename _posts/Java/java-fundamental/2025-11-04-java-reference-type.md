---
title: "Reference Type의 특징"
date: 2025-11-04 00:00:00 +0900
categories: [Java]
tags: [OOP, Java]
---

```java
 int[] origin = {1, 2, 3, 4, 5};
        int[] copied = Arrays.copyOf(origin, origin.length);
        System.out.println(Arrays.toString(copied));

        int[] copied2 = origin;
        System.out.println(Arrays.toString(copied2));

        System.out.println();

        origin[0] = 100;

        System.out.println(Arrays.toString(origin));
        System.out.println(Arrays.toString(copied));
        System.out.println(Arrays.toString(copied2));
```

> 출력 결과 :

```bash
[100, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
[100, 2, 3, 4, 5]
```

- `copied2`는 `origin`의 메모리 주소를 참조 하기 때문에 `origin` 원본 배열의 데이터가 바뀌면 `copied2`의 데이터도 바뀐다.

- `copyOf` 함수를 쓰면 데이터를 복사해 새로운 메모리에 저장 한다.

---

```java
        int[] arrA = {1, 2, 3};
        int[] arrB = {1, 2, 3};

        System.out.println(arrA == arrB);
        System.out.println(Arrays.equals(arrA, arrB));
```

> 출력결과 :

```bash
false
true
```

- 서로 다른 메모리 주소를 `==`으로 비교 하기 때문에 `false`가 나옴
- `Arrays.equals()` 함수로 비교 하면 데이터 값을 비교 한다. `true`가 나옴

---

```java
        int[][] mat = {
        {1, 2}, {3, 4}
        };
        System.out.println(Arrays.toString(mat));
        System.out.println(Arrays.deepToString(mat));// 재귀적으로 데이터를 문자열로 바꿈

```

### 객체도 reference type

- 자바에서 **모든 객체(Object)** 는 **참조(reference) 타입**
- “값을 직접 저장하는 게 아니라, **객체가 존재하는 메모리 주소를 가리키는 참조(포인터 개념)** 를 저장한다”는 뜻
-

```java
Circle c1 = new Circle(10);
Circle c2 = new Circle(20);

Circle c3 = c2;

```

- `new Circle(10)` → 새로운 Circle 객체가 힙(heap) 메모리에 생성됨

- `c1`에는 그 객체의 **주소(reference)** 가 들어감

- `new Circle(20)` → 또 다른 Circle 객체가 생성됨

- `c2`에는 그 두 번째 객체의 **주소(reference)** 가 들어감

- `c3 = c2;` → `c3`가 `c2`의 **주소를 그대로 복사**함 (즉, 같은 객체를 가리킴)

```mathematica
Heap 메모리:
 ┌──────────┐
 │ Circle@A │ radius = 10
 └──────────┘
 ┌──────────┐
 │ Circle@B │ radius = 20
 └──────────┘

Stack (참조 변수):
 c1 ──► Circle@A
 c2 ──► Circle@B
 c3 ──┘ (같은 Circle@B)
```

```java
c1.radius = 100;  // Circle@A의 radius가 100으로 변경됨
c2.radius = 200;  // Circle@B의 radius가 200으로 변경됨
```

## 순서

```java
System.out.println(c1.radius); // 10
System.out.println(c2.radius); // 20
System.out.println(c3.radius); // 20 (c2와 동일 객체)

c1.radius = 100;
c2.radius = 200;

System.out.println(c1.radius); // 100 (c1은 별개 객체)
System.out.println(c2.radius); // 200
System.out.println(c3.radius); // 200 (같은 객체라 동일)

```

```bash
10
20
20
100
200
200
```

| 구분                   | 설명                                              |
| ---------------------- | ------------------------------------------------- |
| **기본형 (primitive)** | int, double, boolean 등. 값을 직접 저장           |
| **참조형 (reference)** | class, array, String, etc. 객체의 **주소**를 저장 |
| `=` 연산               | 객체 복사가 아니라 **참조 주소 복사**             |
| `c3 = c2;`             | c3과 c2가 같은 객체를 가리킴                      |
| 하나를 바꾸면          | 같은 메모리의 값이 바뀌므로 둘 다 영향을 받음     |
