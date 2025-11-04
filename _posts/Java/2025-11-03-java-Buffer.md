---
title: "Java의 Buffer"
date: 2025-11-03 00:00:00 +0900
categories: [Java]
tags: [OOP, Java]
---

```java
package a.basic.practice_for;

public class practice_5 {

    public static void main(String[] args) {
        int num = 24;
        System.out.printf("%d의 약수: ", num);
        for(int i = 1; i <= num; i++) {
            if(num % i == 0){
            System.out.printf(i + " ");
            }
        }
    }
}

```

> 24의 약수: 1 2 3 4 6 8 12 24

- 콘솔 출력(System.out.print, System.out.printf)은 호출될 때마다 바로 화면에 찍히는 게 아니라, 일단 내부 버퍼에 모아두었다가 한꺼번에 출력하는 방식으로 동작

`System.out`은 자바에서 **PrintStream** 객체인데, 이 스트림은 OS의 콘솔(표준 출력)에 직접 쓰기 전에 **일시적으로 데이터를 버퍼(buffer) 에 저장**한다.

- `printf()`나 `print()`는 버퍼에 문자열을 넣음
- 버퍼가 가득 차거나, 줄바꿈 문자(`\n`)가 들어오거나, 프로그램이 끝나거나 `flush()`가 호출될 때 → 그제서야 한꺼번에 화면에 출력

실제로는 printf()가 여러 번 호출되지만, 매번 출력된 문자열이 **버퍼에 쌓였다가, 반복문이 끝날 때 한 번에 화면에 나타나는 것처럼 보이는**것

### 줄바꿈이 있는 경우

```java
System.out.printf("%d\n", i);
```

이때는 `\n`이 들어오면 버퍼가 즉시 flush되어, 각 줄이 바로바로 찍히는 것처럼 보이는것.
