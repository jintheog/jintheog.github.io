---
title: "Prime Number"
date: 2025-11-03 00:00:00 +0900
categories: [Java]
tags: [OOP, Java]
---

소수: **1과 자기 자신 외에는 약수가 없는 수 **

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int num = 17;

       boolean isPrime = true;
       for(int i = 2; i < num; i++) {
           if(num % i == 0) {
            isPrime = false;
            System.out.printf("%d은 소수가 아닙니다.",num);
            break;
           }
       }

       if(isPrime) {
           System.out.printf("%d은 소수입니다.",num);
       }
    }
```

> 2부터 num-1까지 나누어떨어지는 수가 있는지 확인 하면 된다.

- `i <= num-1` 은 `i < num` 과 같다

---

### 소수 판별 최적화

```java
   public static void main(String[] args) {
        int num = 16;

        boolean isPrime = true;
        for(int i = 2; i * i <= num; i++) {
            if(num % i == 0) {
                isPrime = false;
                break;
            }
        }

        if(isPrime) {
            System.out.printf("%d은 소수입니다.",num);
        } else {System.out.printf("%d은 소수가 아닙니다.",num);}
    }
```

- 어떤 수가 소수가 아니면 반드시 **√num 이하의 약수**를 가진다

### e.g. 24

```bash
24 = 2 × 12
24 = 3 × 8
24 = 4 × 6
24 = 6 × 4
24 = 8 × 3
24 = 12 × 2
```

- 이때 앞쪽 수들은 전부 4.9(√24) 이하. 뒤쪽 수들은 √24 이상

- 즉, 약수는 항상 대칭 쌍으로 존재하고, 그중 작은 쪽은 반드시 √num보다 작거나 같다

- `num`이 소수인지 판별할 때, 2부터 √num까지만 나눠봐도 충분하다.

- √num보다 큰 약수가 있다면 그 짝인 작은 약수가 √num보다 작기 때문.

- `Math.sqrt(num)` 는 `i * i <= num` 와 같다
