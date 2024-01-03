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
