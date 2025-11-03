---
title: "향상된 switch문 안에서 쓰는 String.format 함수"
date: 2025-11-03 00:00:00 +0900
categories: [Back, Java, error]
tags: [OOP, Java, error]
---

```java
  public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(true){
            int numOfDay =  sc.nextInt();
            String day = "";
            if(numOfDay == 0){
                System.out.println("프로그램 종료");
                break;
            }

            day = switch(numOfDay){
                case 1 -> "월요일";
                case 2 -> "화요일";
                case 3 -> "수요일";
                case 4 -> "목요일";
                case 5 -> "금요일";
                case 6 -> "토요일";
                case 7 -> "일요일";
                default -> "잘못된 요일";
            };

          String dayOfWeek = switch(numOfDay){
            case 1, 2, 3, 4, 5 -> "%d: %s 은 주중입니다", numOfDay, day;
            case 6, 7 -> "%d: %s은 주말입니다", numOfDay, day; };
        }
    }
```

이 부분이 문제 :

```java
String dayOfWeek = switch(numOfDay){
    case 1, 2, 3, 4, 5 -> "%d: %s 은 주중입니다", numOfDay, day;
    case 6, 7 -> "%d: %s은 주말입니다", numOfDay, day;
};
```

> not a statement error

### 자바의 향상된 switch문(switch expression) 은 “문(statement)”이 아니라 “표현식(expression)” .

- 즉, 값을 반환해야지 명령(출력 등)을 실행하면 안됨.

## 해결 방법 ① — String.format() 사용

printf처럼 문자열 안에서 변수를 넣고 싶으면 String.format()을 써야 함

```java
String dayOfWeek = switch (numOfDay) {
    case 1, 2, 3, 4, 5 -> String.format("%d: %s은 주중입니다", numOfDay, day);
    case 6, 7 -> String.format("%d: %s은 주말입니다", numOfDay, day);
    default -> "잘못된 입력입니다";
};
```

System.out.println(dayOfWeek);

## 해결 방법 ② — switch문(statement)으로 변경하고 직접 출력

만약 “switch문 안에서 바로 출력하고 싶다”면,
switch를 “표현식(expression)”이 아니라 문(statement) 형태로 써야 함

```java
switch (numOfDay) {
    case 1, 2, 3, 4, 5:
        System.out.printf("%d: %s은 주중입니다%n", numOfDay, day);
        break;
    case 6, 7:
        System.out.printf("%d: %s은 주말입니다%n", numOfDay, day);
        break;
    default:
        System.out.println("잘못된 요일입니다");
}
```
