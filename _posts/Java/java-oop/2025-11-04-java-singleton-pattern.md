---
title: "Singleton Pattern"
date: 2025-11-04 00:00:00 +0900
categories: [Java, java-oop]
tags: [OOP, Java]
---

# 싱글톤

- **인스턴스가 오직 1개만 생성 되도록 보장** 하는 디자인 패턴

```java
public class Database {
    // 1. private static 인스턴스
    private static Database instance = null;

    // 2. private 생성자 (외부에서 new 불가)
    private Database() {
        System.out.println("Database 인스턴스 생성");
    }

    // 3. public static getInstance 메서드
    public static Database getInstance() {
        if (instance == null) {
            instance = new Database();
        }
        return instance;
    }

    public void connect() {
        System.out.println("DB 연결");
    }
}

// 사용
// Database db = new Database();  // 컴파일 에러! (생성자가 private)

Database db1 = Database.getInstance();
Database db2 = Database.getInstance();
System.out.println(db1 == db2);  // true (같은 인스턴스)


```

### 가장 중요한 메서드는

```java
   // 3. public static getInstance 메서드
    public static Database getInstance() {
        if (instance == null) {
            instance = new Database();
        }
        return instance;
    }
```

- 최초로 `new Database()`를 통해 새로운 객체를 생성 하려 `Database` 클래스 타입의 `instance` 라는 변수에 대입을 하고 나면 그 후로는 `instance == null` 에 부합 하지 않기때문에 `if`문을 통과 하지 못하고 그냥 반환 하게 됨.

- 즉, 딱 한번만 객체를 생성을 하고 이미 생성 된것이 있으면 계속 그것만 쓰겠다는 뜻.

```java
package c.oop2;

public class Singleton {
    private static Singleton inst = null;

    private Singleton() {
      //private : 외부에서 객체 생성 불가
      // 내부에서 딱 1번만 만들것임.
   }

    public static Singleton getInstance() {
        if (inst == null) {
            inst = new Singleton();
        }
        return inst;
    }
}

```

```java
package c.oop2;

public class SingletonMain {
    public static void main(String[] args) {
//        Singleton s1 = new Singleton();
//        Singleton s2 = new Singleton();
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();

        System.out.println(s1 == s2); //true

    }
}

```

# 장점

1. 메모리 절약 (인스턴스를 1번만 만듬)
2. 전역 접근 가능 (어디서든 `getInstance()` 호출)
3. 공유 리소스 관리 용이 (db연결, 설정, 캐시 등)

# 단점

1. 테스트 어려움 (상태가 공유되어 격리 어려움)
2. 의존성 숨김 (전역 상태로 인한 결합도 증가)
   - 전역 상태로 인한 결합도 증가:
   - 싱글톤은 사실상 **전역 변수(global state)** 랑 비슷하다.
   - 모든 클래스가 Database.getInstance()를 쓰면 다음과 같은 문제가 생김:
     - 테스트하기 어렵다 → 테스트 시 다른 인스턴스로 교체가 불가능함.
     - 모듈 간 결합도 증가 → 하나의 전역 인스턴스에 모두 의존하므로, 그 인스턴스가 바뀌면 전체 코드에 영향이 감.
     - 명시적 의존이 사라짐 → 어디서 어떤 객체를 사용하는지 추적하기 어려움.

결국, 시스템 전체가 전역 객체에 강하게 묶이게(coupled) 됌.

3. 멀티스레드 환경에서 주의 필요

## 사용이 적합한 경우

- 어플리케이션 설정
- 로깅
- DB 연결 풀
- 캐시
- 스레드 풀
- 한번만 해야 하는 일들.
