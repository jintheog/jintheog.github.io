---
title: "Null pointer error"
date: 2025-11-03 00:00:00 +0900
categories: [Java, java-error]
tags: [OOP, Java, error]
---

Main.java:

```java
  // 카테고리 검색
            System.out.println("\n[검색: 전자기기]");
            Product[] electronics = manager.searchProductsByCategory("전자기기");
            for (int i = 0; i < electronics.length; i++) {
                Product p = electronics[i];
                System.out.println((i + 1) + ". " + p.getName() + " - " + p.getPrice() + "원");
            }
```

- 카테고리로 검색 했는데 p가 null인게 섞여 있어서 null pointer 에러가 나왔음
- intellij로 debugging 결과 3번째 배열이 비어있었음.
  - 3번째 상품은 전자기기가 아니라 도서 카테고리리인 상품임.

#### 원인 1

ShopManager.java

```java
  public Product[] searchProductsByCategory(String category){
        Product[]  temp = new Product[50];
        int tempCount = 0;

        for(int i = 0; i < productCount; i++){
            boolean check = products[i].getCategory().equalsIgnoreCase(category);
            if(check){
                temp[i] = products[i];

            }
        }

        return temp;
    }
```

- 처음엔 임시 배열을 반환 해서 null pointer가 나왔었음

#### 원인 2

```java
   public Product[] searchProductsByCategory(String category){
        Product[]  temp = new Product[50];
        int tempCount = 0;

        for(int i = 0; i < productCount; i++){
            boolean check = products[i].getCategory().equalsIgnoreCase(category);
            if(check){
                temp[i] = products[i];
                tempCount++;
            }
        }
        System.out.println(Arrays.toString(temp));
        Product[] searchedProducts = new Product[tempCount];
        for(int i = 0; i < searchedProducts.length; i++){
            searchedProducts[i] = temp[i];
        }
        return searchedProducts;
    }
```

- 결과 (검색 일치 된 상품들 개수)에 맞는 크기의 새로운 배열을 만들어서 저장 후 반환 하고자 했지만 `temp[i] = products[i];` 이 부분에서 i가 순차적으로 0, 1, 2 로 증감 하면 products[i]가 null인게 temp에 담기게 되서 main에서 함수 호출 했을 때 null pointer 에러가 남

#### 해결

```java
temp[tempCount] = products[i];

```

로 바꿔야 함. 이렇게 하면 데이터가 존재하는 것들만 temp 배열안에 쌓임.
