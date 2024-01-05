# JPA

## Annotations

#### @Entity&#x20;

* DB의 테이블을 뜻함

#### @Table&#x20;

* DB 테이블 이름 명시 (테이블명과 클래스명이 동일한 경우 생략 가능)

#### @Id&#x20;

* Primary Key를 뜻함

#### @GeneratedValue&#x20;

* Primary Key의 키 생성 전략
* IDENTITY -> MySQL의 Auto-Increment 방식

#### @Column&#x20;

* DB Column 명시

#### @Transient&#x20;

* @Column과 다르게 테이블에 컬럼으로 생성되지 않는 필드
* 필드에 매핑하지 않고,  DB 저장하지 않고, 조회하지 않음
* 객체에 임시로 어떤 값을 보관하고 싶을 때 사용

#### @Enumerated&#x20;

* 자바 enum 타입 사용
* ORDINAL -> enum에 정의된 순서대로 DB에 저장
* STRING -> enum의 이름 그대로 저장

```java
public enum Authority {
    ADMIN, USER
}
// ORDINAL
ADMIN -> 1, USER -> 2

// STRING
ADMIN -> ADMIN, USER -> USER
```

#### @ Temporal

* 자바 날짜 사용
* Date -> 날짜
* Time -> 시간
* Timestamp -> 날짜와 시간 사용
* @CreationTimestamp -> Insert 시 현재 시간 저장
* @UpdateTimestamp -> Update 시 현재 시간 저장

## JpaRepository 선언 방식

* interface ... extends JpaRepository\<Entity, PK> 와 같이 선언
* 제네릭에는 상속받고자 하는 Entity와 Primary Key의 타입을 지정해주면 됨
* Spring Data JPA 는 자동으로 스프링 빈으로 등록

## Entity 매핑

#### 객체와 테이블 매핑

* @Entity, @Table&#x20;

#### 기본키 매핑

* @Id&#x20;

#### 필드와 컬럼 매핑

* @Column&#x20;

#### 연관관계 매핑

* @ManyToOne, @OneToMany, @JoinColumn, @OneToOne&#x20;

## 연관관계 매핑

* 객체의 참조와 테이블의 외래 키를 매핑하는 것을 의미
* JPA 에서는 연관관계에 있는 상대 테이블의 PK를 멤버변수로 갖지 않고, 엔티티 객체 자체를 참조
* 단순히 참조하는 것만으로는 연관관계를 맺을 수 없음

### 방향

#### 단방향

* 두 엔티티가 관계를 맺을 때, 한 쪽의 엔티티만 참조하는 것을 의미

#### 양방향

* 두 엔티티가 관계를 맺을 때, 양 쪽이 서로를 참조하는 것을 의미

### 다중성

* ManyToOne(N:1), OneToMany(1:N), OneToOne(1:1), ManyToMany(N:M)
* ex) 하나의 Team은 여러 Member를 구성원으로 가지고 있으므로 Team 입장에서 1:N 관계이고, Member 입장에선 하나의 Team에 속하는 것이니 N:1 관계
* 즉, 어떤 엔티티를 중심으로 상대를 보는지에 따라 다중성이 달라짐

### 연관관계의 주인

* 객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해야함
* 연관관계를 갖는 두 테이블에서 외래키를 갖는 테이블이 연관관계의 주인이 됨
* 연관관계의 주인만이 외래 키를 관리(등록, 수정, 삭제)할 수 있고, 주인이 아닌 엔티티는 읽기만 가능

### @ManyToOne&#x20;

* 단방향 매핑이므로 한쪽의 엔티티가 상대 엔티티를 참조할 때 사용
* @JoinColumn(name = "TEAM\_ID")는 외래키를 매핑할 때 사용
* name 속성은 매핑할 외래키 이름을 지정
* Member 엔티티의 경우 Team 엔티티의 id 필드를 외래키로 가짐
* Member와 Team은 단방향 관계를 가지므로 Member에서 Team의 정보를 가져올 수 있음
* 만약 Team에서 Member 정보를 가져오고 싶다면 양방향 관계를 맺으면 되는데, N:M의 관계라기 보다는 N:1, 1:N을 해주는 느낌 (@OneToMany)

```java
@Entity
@Getter @Setter
public class Member {
     @Id    
     @GeneratedValue(strategy = GenerationType.IDENTITY)    
     private Long id;     
     @Column(nullable = false)    
     private String username;     
     @ManyToOne    
     @JoinColumn(name = "TEAM_ID")    
     private Team team;
} 

@Entity
@Getter @Setter
public class Team {     
     @Id    
     @GeneratedValue(strategy = GenerationType.IDENTITY)    
     private Long id;     
     @Column(nullable = false)    
     private String name;
}
```

### @OneToMany&#x20;

* Team은 Member를 리스트로 가지고, 연관관계의 주인을 정하기 위해 mappedBy 속성을 사용
* 연관관계의 주인을 정하는 방법은 mappedBy 속성을 지정하는 것
* 주인은 mappedBy를 사용하지 않고 @JoinColumn을 사용
* 주인이 아닌 엔티티 클래스는 mappedBy를 사용하여 주인을 정할 수 있음
* 주인은 mappedBy 속성을 사용할 수 없으므로 연관관계의 주인이 아닌 Team 엔티티에서 members 필드에 mappedBy속성으로 Member 테이블의 team 필드를 지정

```java
@Entity
@Getter @Setter
public class Member {
     @Id    
     @GeneratedValue(strategy = GenerationType.IDENTITY)    
     private Long id;     
     @Column(nullable = false)    
     private String username;     
     @ManyToOne    
     @JoinColumn(name = "TEAM_ID")    
     private Team team;
} 

@Entity
@Getter @Setter
public class Team {     
     @Id    
     @GeneratedValue(strategy = GenerationType.IDENTITY)    
     private Long id;     
     @Column(nullable = false)    
     private String name;
     
     // 추가된 부분
     @OneToMany(mappedBy = "team")
     private List<Member> members = new ArrayList<>();
}
```

### @OneToOne&#x20;

* 한 유저는 한 개의 블로그만 생성할 수 있다고 하면 관계는 일대일
* 유저 엔티티 입장에서 블로그의 @Id를 외래키로 가져서 블로그를 참조할 수 있고, 블로그가 외래키를 가져서 유저를 참조하는 것도 가능
* 만약 한 유저가 여러 개의 블로그를 확장하는 것을 고려한다면 (OneToMany) 블로그에서 유저의 외래키를 가지는 것이 좋을 것이고, 유저를 조회했을 때 블로그 목록도 나오는 것을 고려한다면 유저에서 갖는 것이 좋다고 볼 수 있음

```java
// 유저에 외래키를 두는 방식
@Entity
@Getter @Setter
public class User {        
    @Id    
    @GeneratedValue(strategy = GenerationType.IDENTITY)    
    private Long no;        
    @Column(nullable = false)    
    private String id;        
    @OneToOne    
    @JoinColumn(name = "blog_no")    
    private Blog blog;
} 
@Entity
@Getter @Setter
public class Blog {     
    @Id    
    @GeneratedValue(strategy = GenerationType.IDENTITY)    
    private Long no;     
    @Column(nullable = false)    
    private String address;
}

```

### @ManyToMany&#x20;

* 실무에서 사용하지 않음
* 중간 테이블이 숨겨져있기 때문에 자신도 모르는 복잡한 쿼리가 발생하는 경우가 생길 수 있기 때문
* 다대다로 생성된 테이블은 두 객체 테이블의 외래 키만 저장되기 때문에 문제가 될 확률이 높음
* 따라서 다대다를 일대다, 다대일로 유연하게 풀어내야 함
