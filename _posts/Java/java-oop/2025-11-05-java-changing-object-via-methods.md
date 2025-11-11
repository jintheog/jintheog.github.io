---
title: "How Object changes via other methods in other classes"
date: 2025-11-05 00:00:00 +0900
categories: [Java, java-oop]
tags: [OOP, Java]
---

### Library 클래스에 있는 `applyDiscountToAll` 함수에서 `books[i]`에다가 Book 클래스에 있는 `applyDiscount` 함수를 쓰면 applyDiscount 함수가 실행 되는데 그게 정확히 어떻게 `books` 배열 안에 있는 객체 하나 하나의 price라는 속성에 접근해서 `applyDiscount` 함수의 로직을 적용 하여 데이터들을 변경 시키는 걸까?

> `Library Class`

```java
package c.oop2.practice;

public class Library {
    // TODO: 필드 선언
    // - books (Book[]): 책 배열
    // - bookCount (int): 현재 책의 개수
    Book[] books;
    int bookCount;

    // TODO: 생성자
    // Library(int capacity)
    // - books 배열을 capacity 크기로 생성
    // - bookCount를 0으로 초기화
    public Library(int capacity) {
        this.books = new Book[capacity];
        this.bookCount = 0;
    }

    // TODO: applyDiscountToAll(int percent) 메서드
    // - 모든 책에 percent% 할인 적용
    public void applyDiscountToAll(int percent){
        for(int i = 0; i < bookCount; i++) {
            books[i].applyDiscount(percent);
        }
    }
}

```

> `Book Class`

```java
public class Book {
    // TODO: 필드 선언
    // - title (String): 제목
    // - author (String): 저자
    // - price (int): 가격
    // - isbn (String): ISBN
    String title;
    String author;
    int price;
    String isbn;

    // TODO: applyDiscount(int percent) 메서드
    // percent% 할인 적용 (예: 10 입력 시 10% 할인)
    public void applyDiscount(int percent){
        price = price - (price * percent / 100);
    }
}
```

### 1. `books[i]`는 Book 객체의 참조(reference)이다

```java
Book[] books = new Book[10];
books[0] = new Book("홍길동전", "허균", 15000);
```

메모리에서:

- `books` 배열은 `Book` 객체들의 주소(참조)를 저장한다
- `books[0]`는 실제 `Book` 객체가 있는 메모리 위치를 가리킨다

```bash
books 배열:
[0] → Book 객체 1 (메모리 주소: 0x1234)
[1] → Book 객체 2 (메모리 주소: 0x5678)
[2] → Book 객체 3 (메모리 주소: 0x9abc)

Book 객체 1 (0x1234):
  ├─ title: "홍길동전"
  ├─ author: "허균"
  └─ price: 15000
```

### 2. books[i].applyDiscount(10) 호출

```java
books[0].applyDiscount(10);

// books[0]가 가리키는 Book 객체에서 applyDiscount 메서드 실행
// 즉, 0x1234 주소에 있는 Book 객체의 applyDiscount 메서드 호출
```

### 3. applyDiscount 메서드 내부에서

```java
public void applyDiscount(int percent){
    price = price - (price * percent / 100);
    // 여기서 price는 this.price와 동일
    // this는 메서드를 호출한 그 객체 자신을 가리킴
}
```

완전히 풀어쓰면:

```java
public void applyDiscount(int percent){
    this.price = this.price - (this.price * percent / 100);
    // this = books[0]이 가리키는 그 Book 객체
    // 따라서 그 객체의 price 필드를 직접 수정함
}
```

### 4. 실제 동작 예시

```java
// Library.java
public void applyDiscountToAll(int percent){
    for(int i = 0; i < bookCount; i++) {
        books[i].applyDiscount(percent);
    }
}
```

i = 0일 때:

```java
books[0].applyDiscount(10);
// → books[0]이 가리키는 Book 객체로 가서
// → 그 객체의 applyDiscount 메서드 실행
// → 그 객체의 price 필드를 수정 (15000 → 13500)
```

i = 1일 때:

```java
books[1].applyDiscount(10);
// → books[1]이 가리키는 다른 Book 객체로 가서
// → 그 객체의 applyDiscount 메서드 실행
// → 그 객체의 price 필드를 수정 (18000 → 16200)
```

### 핵심 개념

1. 배열 요소 = 객체 참조: books[i]는 실제 Book 객체의 메모리 주소를 저장
2. 메서드 호출 = 해당 객체에서 실행: books[i].applyDiscount()는 books[i]가 가리키는 그 특정 객체의 메서드를 실행
3. this 키워드: 메서드 내부에서 필드를 참조하면 자동으로 자기 자신(this)의 필드를 수정

따라서 배열을 반복하면서 각 요소에 메서드를 호출하면, 각 객체가 자신의 데이터를 수정하게 된다.
