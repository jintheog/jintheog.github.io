---
title: " @Repository"
date: 2025-11-18 00:00:00 +0900
categories: [Spring, MVC]
tags: [Spring, MVC, annotation]
---

### @Repository가 Repository 클래스를 스프링 “빈(Bean)”으로 등록해주기 때문에, Spring이 이 객체를 단 하나만 만들어서 모든 클래스에 자동으로 주입(DI)해 준다. 그래서 new로 만들 필요가 없다.

## 1. @Repository의 진짜 역할은 “Bean 등록”

- `@Repository`는 다음과 같은 역할을 함:

#### 1. Spring에게 "이 클래스는 Bean으로 관리해줘"라고 표시

- 즉, Spring Container가 어플리케이션 시작할 때:

```java
new ToDoRepository()
```

를 대신 해줌. (IoC Container가 객체 생성)

#### 2. 이 Bean을 “싱글톤(singleton)”으로 관리

즉:

- 애플리케이션 전체에서 단 하나의 ToDoRepository 인스턴스만 존재

- Controller, Service, CommandLineRunner 등 어디서든 같은 Repository 객체를 공유해서 사용

**=> new ToDoRepository()를 여러 번 만들면 저장소가 여러 개 생기지만,Spring이 빈으로 만들면 저장소는 딱 하나만 존재한다.**

## 2. 그래서 이런 코드가 가능해지는 것

```java
public ToDoController(ToDoRepository toDoRepository) {
    this.toDoRepository = toDoRepository;
}
```

여기서 `toDoRepository`는 **Spring이 자동으로 넣어주는 Bean**이다.

new를 쓰지 않는데도 ToDoRepository를 사용할 수 있는 이유는 **Spring이 ToDoRepository 객체를 만들어놓고, 필요한 클래스에 생성자 주입(DI)으로 전달해주기 때문.**

### 이걸 ‘의존성 주입(Dependency Injection)’이라고 함

## 3. CommandLineRunner에서도 같은 Repository가 자동으로 들어옴

```java
@Bean
public CommandLineRunner init(ToDoRepository toDoRepository) {
    return args -> {
        toDoRepository.save(new ToDoDTO(null, "study", "JAVA", false));
    };
}
```

여기서도 같은 **Repository Bean 인스턴스가 주입(DI)** 됨.

즉, Controller에서 쓰는 저장소 == 여기에서 쓰는 저장소 데이터가 계속 유지되는 이유가 이 때문.

## 4. @Repository가 “대신 해주는 것”을 정확히 말하면

### ✔ IoC (제어의 역전)

new로 객체를 생성하는 **제어권을 개발자가 아닌 Spring에게 넘김**

### ✔ DI (의존성 주입)

원하는 곳에 자동으로 Repository 객체를 넣어줌

### ✔ 싱글톤 관리

Repository 객체가 **단 하나만 존재하도록 관리**

### ✔ 예외 변환 기능

(@Repository는 JDBC 예외를 스프링 예외로 자동 변환해주는 기능도 있음)

---

### 왜 new ToDoRepository()를 쓰면 안 되는가?

```java
ToDoRepository r1 = new ToDoRepository(); // Controller
ToDoRepository r2 = new ToDoRepository(); // CommandLineRunner
```

이러면 저장소가 2개 생김.

- r1에 저장한 데이터는 r2에서 보이지 않고

- r2에서 만든 데이터는 Controller에서 보이지 않음

**= 애플리케이션이 정상적으로 동작하지 않음**

Spring Bean으로 등록하면 해결:

```bash
[Spring Container]
ToDoRepository bean 인스턴스 하나만 생성   ← 여기 저장소가 딱 하나
     ↓
Controller 주입
CommandLineRunner 주입
Service 주입
필요한 모든 곳에서 동일 객체 이용

```

---

#### ✔ @Repository는 “이 클래스를 Bean으로 만들어줘”라는 표시

#### ✔ Spring이 애플리케이션 시작 시 ToDoRepository 객체를 단 한 번만 생성

#### ✔ Controller나 CommandLineRunner 등 여러 곳에 같은 Repository 인스턴스를 DI 해줌

#### ✔ 그래서 new ToDoRepository() 할 필요가 없음

#### ✔ 저장소(Map)도 하나만 유지되기 때문에 데이터가 공유됨
