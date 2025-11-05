---
title: "삼각형 변의 조건식에 &&을 써야 하는 이유"
date: 2025-11-03 00:00:00 +0900
categories: [Java, java-error]
tags: [OOP, Java, error]
---

## 문제 4: 삼각형 유효성 검사

세 변의 길이를 입력받아 삼각형을 만들 수 있는지 검사하세요.

**삼각형 조건:**

- 세 변의 길이가 모두 양수
- 가장 긴 변 < 나머지 두 변의 합

**입력:**

```java
int a = 3, b = 4, c = 5;
```

**예상 출력:**

```
삼각형을 만들 수 있습니다.
```

### 두번째 조건식에 && 연산자가 아닌 || 연산자를 썼을 경우 로직 오류:

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("a변의 길이 입력: ");
        int a = sc.nextInt();
        System.out.print("b변의 길이 입력: ");
        int b = sc.nextInt();
        System.out.print("c변의 길이 입력: ");
        int c = sc.nextInt();

        boolean valid = (a>0&&b>0&&c>0) && ((a+b)>c||(b+c)>a||(a+c)>b);
        if (valid) {
            System.out.println("삼각형을 만들 수 있습니다");
        } else System.out.println("삼각형을 만들 수 없습니다");
    }
```

```
a변의 길이 입력: 1
b변의 길이 입력: 2
c변의 길이 입력: 3
삼각형을 만들 수 있습니다
```

- 2번째 조건에 부합하지 않기 때문에 삼각형을 만들 수 없다고 나와야 하는데 만들 수 있다고 나옴. 가장 긴 변이 a일 수도 b일수도 c일 수도 있다고 생각 함.
  - 하지만 이 조건은 “세 변 중 어느 하나라도 이 조건을 만족하면 된다”가 아니라, 세 변 전체가 동시에 삼각형을 이룰 수 있는 하나의 완전한 조합이어야 하기 때문

> “가장 긴 변이 무엇이든 간에, 그 ‘가장 긴 변’은 반드시 나머지 두 변의 합보다 작아야 한다.” -> 모든 조합에서 이 부등식이 유지되어야 함

## e.g. :

```text
a=1, b=2, c=3
```

OR 조건:

```java
(a+b>c) || (a+c>b) || (b+c>a)
```

계산하면:

```bash
(1+2>3) → false
(1+3>2) → true
(2+3>1) → true
```

> false || true || true → true

- 그래서 삼각형이 된다고 출력 ❌

하지만 실제로는 세 변이 일직선이라 삼각형 불가능

🔹 올바른 조건 (AND)

삼각형은 세 변이 모두 서로 맞물려야만 완성되기 때문에,
세 부등식이 **전부 동시**에 참이어야 한다 👇

```java
(a+b>c) && (a+c>b) && (b+c>a)
```

위 조건이면:

```bash
(1+2>3) → false
(1+3>2) → true
(2+3>1) → true
false && true && true → false
```

> 삼각형 불가능 ✅

OR → “하나라도 맞으면 통과”
→ 세 변 중 일부만 조건을 만족해도 OK → ❌ 잘못된 판단

AND → “모두 맞아야 통과”
→ 세 변이 실제로 삼각형을 형성해야 함 → ✅ 올바른 판단
