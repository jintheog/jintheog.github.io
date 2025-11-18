---
title: "Controller annotation @Controller"
date: 2025-11-18 00:00:00 +0900
categories: [Spring, MVC]
tags: [Spring, MVC, annotation]
---

# @Controller 어노테이션을 쓰면

- 생성자가 없을 경우 기본 생성자를 사용하려 객체 생성을 대신 해준다.
- 생성자를 만들었다면 그걸 이용하려 객체 생성을 대신 해준다

```java
@Controller
public class ToDoController {
    private final ToDoRepository toDoRepository;

    public ToDoController(ToDoRepository toDoRepository) {
        this.toDoRepository = toDoRepository;
    }
}
```

### Spring이 @Controller를 보고 스프링 빈(Bean)으로 등록해서 DI 컨테이너가 객체를 생성한다

- 객체를 만드는 건 @Controller가 아니라 Spring IoC/DI 컨테이너다.

# @Controller는 다음 역할을 한다:

1. Spring이 이 클래스를 스캔해서 스프링 빈으로 등록

- @Controller는 @Component의 특수화 어노테이션

- Component Scan에 의해 Bean 등록

- 이때 객체 생성은 Spring IoC Container가 담당

2. 컨트롤러 클래스를 Spring MVC 요청 처리 대상으로 등록

- URL 매핑(@RequestMapping, @GetMapping 등)을 활성화
- DispatcherServlet이 이 클래스를 핸들러로 인식

#### 즉, @Controller는 “이 클래스가 컨트롤러 역할을 한다”는 표시 역할을 하는 것이다.

---

# 생성자 관련 정확한 동작

### 상황 1: 생성자 없음

- Spring이 기본 생성자로 객체를 생성 → Bean 등록

### 상황 2: 생성자 있음 (예: Repository 주입)

- Spring은 **생성자에 필요한 타입의 Bean을 찾아서 자동 주입(Dependency Injection)** → 그 생성자를 이용해서 객체를 만들고 등록

### ➡️ 생성자를 호출해서 객체 만드는 건 @Controller가 아니라 Spring Container임

- “@Controller는 해당 클래스를 스프링 MVC의 컨트롤러로 등록하고, 스프링이 이 클래스를 Bean으로 만들도록 표시하는 어노테이션이다.”

- “Bean 생성과 의존성 주입은 Spring IoC 컨테이너가 담당한다.”

- ➡️ **어노테이션은 객체를 생성하는 능력이 없다.
  객체 생성은 항상 Spring IoC 컨테이너의 역할이다.**

# 정리

### @Controller의 정확한 역할

- Spring Component Scan에 의해 Bean 등록 대상임을 표시

- 스프링 MVC 요청 매핑 대상 클래스임을 표시

- DispatcherServlet이 이 클래스를 Handler로 인식하도록 함

### 객체 생성 주체

#### ➡️ Spring IoC Container

### 생성자 주입이 되는 이유

#### ➡️ Spring이 Bean 생성 시 생성자에 필요한 의존성을 자동으로 주입해주기 때문
