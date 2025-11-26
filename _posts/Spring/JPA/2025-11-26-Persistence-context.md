---
title: "Persistence Context 영속성 컨텍스트"
date: 2025-11-26 00:00:00 +0900
categories: [Spring, JPA]
tags: [Spring, JPA]
---

# Persistence Context : JPA의 핵심

- JPA가 "마법처럼 보이는 기능"을 제공 하는 이유가 전부 여기서 나옴

**영속성 컨텍스트: JPA가 엔티티를 저장하고 관리하는 1차 캐시(저장소)**

- 엔티티를 넣어두고
- 상태 변화를 추적하고
- DB 반영 시점을 조절하고
- 동일한 엔티티는 1개만 존재하도록 보장해줌

**JPA내부의 (눈에 보이지 않는) 저장소**

---

# 엔티티를 영속성 컨텍스트에 넣는 순간 "JPA가 관리 시작"

```java
Post post = postRepository.findById(1L).orElseThrow();
```

이 순간 JPA는 이렇게 함:

- DB에서 Post(1번)를 가져옴

- 영속성 컨텍스트에 저장함

- 이후부터 post는 JPA가 관리하는 “관리 객체(Managed Entity)”가 됨

### 2. 영속성 컨텍스트는 1차 캐시를 가진다

같은 트랜잭션에서 다시 조회 하는 경우 :

```java
Post post1 = repo.findById(1L);
Post post2 = repo.findById(1L);

System.out.println(post1 == post2); // true
```

왜냐면:

- 첫 번째 조회: DB에서 가져와서 캐시에 저장

- 두 번째 조회: DB로 안 가고 캐시에서 꺼냄

- 그래서 **DB 조회 쿼리가 한 번만 나감**

### 3. 더티 체크(Dirty Checking)

엔티티 값을 바꾸는 경우 :

```java
post.setTitle("hello");
```

이러고 트랜잭션 끝나면 JPA는 자동으로:

- “어? title 값 변경됐네?”

- UPDATE 쿼리 자동 생성

- DB에 반영

즉,

> 수정 쿼리를 직접 안 써도 JPA가 알아서 쓴다.

### 4. 쓰기 지연(Write-Behind)

트랜잭션 도중 여러 번 수정 하는 경우 :

```java
post.setTitle("hello");
post.setTitle("bye");
post.setTitle("final");

```

JPA는:

- “계속 바꾸네? 업데이트 3번 안 날릴게”

- “트랜잭션 끝날 때 마지막 값만 DB에 반영할게”

즉:

> 업데이트 쿼리는 트랜잭션 마지막에 한 번만 나감.

### 5. 동일성(Identity) 보장

같은 PK(1L)의 엔티티는 트랜잭션 안에서 항상 동일 객체.

```java
Post p1 = repo.findById(1L);
Post p2 = repo.findById(1L);

p1 == p2 // true
```

DB에서 두 번 가져오지 않음.

### 6. 연관관계 관리도 돕는다 (지연 로딩 등)

e.g. :

```java
post.getComments()
```

post가 영속성 컨텍스트에 있으면 댓글을 필요할 때만 DB에서 조회하는 **Lazy Loading**도 여기서 가능해짐.

# 영속성 컨텍스트가 하는 일 5가지

| 기능           | 설명                                      |
| -------------- | ----------------------------------------- |
| 1차 캐시       | 같은 엔티티 중복 조회 방지                |
| 동일성 보장    | 같은 트랜잭션에서 같은 객체 사용          |
| 쓰기 지연      | UPDATE/INSERT를 트랜잭션 끝에 모아서 보냄 |
| 더티 체크      | 값만 바꾸면 UPDATE 자동                   |
| 지연 로딩 지원 | 필요한 시점에 연관 엔티티 로딩            |

# 영속성 컨텍스트 = 트랜잭션 동안 엔티티를 담아두는 스마트 캐비닛

- 캐비닛에 넣어두면 JPA가 알아서 변화 감지

- 문서(엔티티)를 바꾸면 자동으로 저장(UPDATE)

- 같은 키의 문서는 한 개만 존재

- 필요할 때 꺼내서 사용

# 장점

- 매번 DB SELECT/UPDATE 수동으로 안 해도 됨

- 자바 객체만 다뤄도 DB가 동기화됨

- 성능도 자동으로 최적화됨

- 연관관계 처리도 쉬움

---

- 영속성 컨텍스트에 들어가는 3가지 순간

1. `save()` 또는 `persist()` 호출 할때

```java
Post post = new Post("title", "content");
postRepository.save(post);
```

혹은 JPA EntityManager 사용시:

```java
em.persist(post);
```

이 순간:

- Post 엔티티가 영속성 컨텍스트에 들어감

- JPA가 관리 시작

- ID가 생성(GenerationType에 따라)

- 트랜잭션 끝날 때 INSERT 쿼리 나감

2. DB에서 조회할 때 (`findById`, `getOne`, JPQL select)

```java
Post post = postRepository.findById(1L).orElseThrow();
```

이 때도 자동으로 들어간다.

과정:

1. DB에서 조회
2. 엔티티 생성
3. 영속성 컨텍스트에 저장
4. 반환

그래서 같은 트랜잭션 안에선:

```java
repo.findById(1L) == repo.findById(1L)  // true
```

이유: 두 번째 조회는 DB가 아니라 영속성 컨텍스트(1차 캐시)에서 가져오기 때문.

3. JPQL이나 QueryDSL로 조회했을 때도 동일하게 영속 상태로 들어감

```java
List<Post> posts = em.createQuery("select p from Post p", Post.class)
                     .getResultList();
```

이 때 조회된 모든 결과는 **자동으로 영속성 컨텍스트에 넣어짐.**

→ 그래서 바로 더티 체크 가능.

---

# ❌ 영속성 컨텍스트에 들어가지 않는 경우

#### ❌ 1) DTO로 조회하는 경우

```java
select new com.example.PostDto(p.id, p.title) from Post p
```

이건 엔티티가 아니라 DTO라서 영속성 컨텍스트에 들어가지 않음.

#### ❌ 2) 엔티티를 new 로 생성만 한 경우

```java
Post post = new Post("hello", "world");
```

이건 **비영속** 상태.

영속성 컨텍스트에 들어가려면 **persist/save** 해야 함.

#### ❌ 3) @Transactional 밖에서 조회한 엔티티

- 트랜잭션이 없으면 영속성 컨텍스트도 없음.
- 조회는 되지만 관리되지 않는 상태가 될 수 있음.

---

# 엔티티의 생명주기

| 상태                   | 설명                                              |
| ---------------------- | ------------------------------------------------- |
| **비영속 (Transient)** | new 했지만 JPA가 모름                             |
| **영속 (Persistent)**  | save/persist/find 로 들어간 상태 (JPA 관리)       |
| **준영속 (Detached)**  | 영속 -> 떼낸 상태 (`em.detach`, 트랜잭션 종료 등) |
| **삭제 (Removed)**     | 영속 상태 + remove() 호출                         |

# e.g. 흐름

```java
@Transactional
public void example() {
    Post post = new Post("A", "B"); // 비영속
    em.persist(post);               // 영속 시작
    post.setTitle("C");             // 더티 체크 대상
    // 트랜잭션 끝 ⟶ UPDATE 자동 반영
}
```

### 영속성 컨텍스트에 어떻게 넣나?

- save/persist 하거나, DB에서 조회하면 자동으로 들어간다.

---

# 더티 체크 Dirty Checking

- JPA가 엔티티의 변경을 자동으로 감지해서 UPDATE쿼리를 알아서 날려주는 기능
- 예 : setter만 바꿨는데 db가 자동으로 수정
- 더티체크 = 영속성 컨텍스트 안에 있는 엔티티의 변경점을 감지하고 트랜잭션 종료 시 자동으로 UPDATE 쿼리를 실행 하는것

### 트랜잭션 시작

```ini
post = findById(1)  → DB에서 가져옴 → 영속성 컨텍스트에 저장
```

영속성 컨텍스트에는 이렇게 기록됨:

- 엔티티의 **원본 스냅샷(초기값)**
- 엔티티의 **현재 상태**

### 엔티티 값 변경

```java
post.setTitle("새 제목");
```

- 이 순간 DB 쿼리 안 나감.
- 그냥 자바 객체 값만 바뀜.

- JPA는 딱히 반응하지 않는 것처럼 보이지만, 영속성 컨텍스트 안에서는 이런 일들이 일어남:

```ruby
원본 값: "기존 제목"
현재 값: "새 제목"
→ “바뀌었네?” 하고 체크해둠
```

### 트랜잭션 종료 (`commit`)

- JPA는 트랜잭션 끝나기 전에 자동으로:
  - 원본 스냅샷 vs 현재 상태 비교
  - 변경된 필드만 UPDATE 쿼리 생성
  - DB에 반영
    즉, 개발자는 UPDATE 문을 직접 쓰지 않아도 됨.

# e.g.

```java
@Transactional
public void updatePost() {
    Post post = postRepository.findById(1L).orElseThrow();
    post.setTitle("hello"); // UPDATE 안 나감
    post.setContent("world"); // UPDATE 안 나감
}
// 여기서 트랜잭션 끝나면 자동 UPDATE
```

JPA가 자동으로 만든 쿼리:

```sql
UPDATE post
SET title = 'hello', content = 'world'
WHERE id = 1;
```

# 더티 체크가 언제 동작하는 조건

더티 체크는 다음 조건을 만족해야 함:

1. 엔티티가 **영속 상태(Persistent)** 여야 한다
   → 비영속(new로 만들기만 한 객체)는 더티체킹 대상 아니야

2. 트랜잭션이 있어야 한다
   → `@Transactional` 없으면 더티체킹 안 됨

3. 플러시(flush) 시점에 동작한다
   → 트랜잭션 종료 시 자동 flush 호출됨

---

# ❌ 더티 체크가 안 되는 경우

- 엔티티를 DTO로 바꾼 뒤 수정 → 엔티티가 아니라서 더티체킹 X

- detach() 해서 준영속 상태로 만들면 더티체킹 X

- 트랜잭션 없이 조회하면 더티체킹 X

- EntityManager.clear() 해서 컨텍스트를 비우면 더티체킹 X

### ✔ 개발자가 UPDATE 쿼리를 안 써도 됨

비즈니스 로직이 깔끔해짐.

### ✔ 변경된 필드만 UPDATE

효율적이고 퍼포먼스 좋음.

### ✔ 복잡한 도메인 모델에서도 객체 그래프만 수정하면 OK

쿼리 관리가 사라짐 → 생산성 폭증.
