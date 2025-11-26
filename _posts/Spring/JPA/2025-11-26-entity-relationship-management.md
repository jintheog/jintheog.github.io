---
title: "연관관계 처리 entity relationship mapping (Association)"
date: 2025-11-26 00:00:00 +0900
categories: [Spring, JPA]
tags: [Spring, JPA]
---

# 연관 관계 처리 : JPA가 엔티티들 사이의 관계(1:N, N:1, N:N, 1:1)를 자동으로 관리해주는 기능

- Post → Comment,
- User → Order,
- Team → Member
  같은 "테이블 간 관계"르 ㄹ객체에서도 자연스럽게 연결해서 사용 할 수 있게 만드는 것.

- 연관관계 처리 = DB 테이블 관계를 자바 객체 관계로 자동으로 이어주고, 그 관계를 편하게 사용할 수 있게 도와주는 JPA의 기능.

DB에서는 관계를 FK로 관리함

e.g. Comment 테이블
| comment_id | post_id(FK) | content |
| ---------- | ----------- | ------- |
자바 객체에서는 이런 관계가 그냥 숫자로만 존재하면 불편하다

```java
Comment comment = ...
Long postId = comment.getPostId(); // 이걸로 끝? 너무 불편
```

그래서 **JPA는 객체끼리도 관계를 직접 맺을 수 있게** 만들어줌.

---

# JPA의 연관관계 처리 기능들

### 1) 객체 그래프 탐색

```java
comment.getPost().getTitle();
```

- 이런 식으로 객체 간을 **점(.)으로 계속 탐색 가능**.

- JDBC/MyBatis에서는 절대 불가능 → 직접 JOIN 쿼리 작성해야 함.

### 2) Mapping Annotation 제공

| 관계 | JPA 애너테이션 |
| ---- | -------------- |
| N:1  | `@ManyToOne`   |
| 1:N  | `@OneToMany`   |
| 1:1  | `@OneToOne`    |
| N:N  | `@ManyToMany`  |

이걸로 엔티티끼리 직접 연결.

### 3) 지연 로딩(Lazy Loading)

연관 객체가 필요할 때만 SELECT 실행.

```java
Post post = postRepository.findById(1L);
post.getComments(); // 이 순간에 SELECT comments...
```

쿼리 최적화 자동.

### 4) 영속성 전이(Cascade)

부모 저장하면 자식도 자동 저장.

```java
em.persist(post); // comments도 자동 persist 되는 옵션 가능
```

### 5) 연관관계 편의 메서드 제공

양방향 관계를 안전하게 설정하기 위한 메서드.

```java
public void addComment(Comment c) {
    comments.add(c);
    c.setPost(this);
}
```

### 6) 연관관계 수정도 자동 추적 (더티체킹 적용)

```java
post.getComments().remove(comment);
```

이렇게만 해도:

- comment의 FK(post_id)가 null로 업데이트되거나

- 삭제되거나

JPA가 알아서 처리.

---

# e.g.

### Entity

#### Post

```java
@OneToMany(mappedBy = "post")
private List<Comment> comments = new ArrayList<>();
```

#### Comment

```java
@ManyToOne
@JoinColumn(name="post_id")
private Post post;
```

이제 Comment로부터 Post를 이렇게 접근:

```java
comment.getPost().getTitle();
```

Post도 Comment 리스트 접근 가능:

```java
post.getComments().size();
```

이게 “연관관계 처리”의 핵심.

---

# ❌ 왜 JDBC/MyBatis는 연관관계 처리가 없을까?

JDBC/MyBatis는:

- 객체를 관리하지 않음

- 연관 객체 자동 조회 없음

- 자동 지연로딩 없음

- 더티체킹 없음

결국 모든 관계 처리:

- JOIN 쿼리 직접 작성

- VO/DTO 조립 직접

- 관계 수정 시 UPDATE 문 직접 작성

연관관계 관리가 개발자 책임.

# JPA 연관관계 처리의 가치

| 기능             | 설명                                        |
| ---------------- | ------------------------------------------- |
| 객체 그래프 탐색 | 객체끼리 직접 연결해서 탐색 가능            |
| 지연 로딩        | 필요할 때만 조회                            |
| 캐시 활용        | 동일 객체 중복 조회 없음                    |
| 가독성           | SQL 로직 사라지고 도메인 로직 중심          |
| 유지보수         | DB 스키마 바뀌어도 코드 구조 크게 안 망가짐 |
