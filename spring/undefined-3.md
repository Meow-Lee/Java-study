---
description: Chapter 6
---

# 데이터 파고들기

* 스프링 데이터의 미션은 기본적인 데이터 저장의 특수한 속성을 유지하면서 데이터에 액세스하는 친숙하고 일관된 스프링 기반 프로그래밍 모델을 제공하는 것
* 즉, 어떤 데이터베이스 엔진이나 플랫폼을 사용하든 간에 스프링 데이터의 목표는 개발자가 가능한 한 간단하고 강력하게 데이터에 액세스하게 하는 것

## 엔티티 정의

* 어떤 형태로든 데이터를 다루는 거의 모든 경우, 도메인 엔티티가 존재
* 개발자가 특정한 사용 사례에서 어느 수준의 복잡성을 사용하기로 정하든 간에, 어떤 형태로든 첫 단계는 적용가능한 데이터를 처리하는데 사용할 도메인 클래스를 정의하는 일
* DDD(Domain-Driven Design)(도메인 주도 설계)
* 도메인 클래스는 연관성과 중요성이 다른 데이터와 독립적인 기본 도메인 엔티티
* 다른 도메인 엔티티와 관련이 없다는 뜻이 아니라 다른 엔티티와 연결되지 않은 때에도 단독으로 존재하고 그 자체로 의미 있다는 뜻
* 도메인 클래스를 우선 하나 정의한 후에 데이터 사용 범위, 클라이언트가 사용하는 외부 API, DB 종류를 고려해 데이터베이스와 추상화 수준을 정하는데, 스프링 생태계에선 이런 작업을 할 때 보통 템플릿과 repository 중 하나를 사용

## 템플릿 지원

* 충분히 높은 수준의 일관된 추상화를 제공하기 위해 스프링 데이터는 대부분의 다양한 데이터 소스에 Operations 타입의 인터페이스를 정의
* Operations 타입 인터페이스 (MongoOperations, RedisOperations, CassandraOperations)는 최선의 유연성을 위해 바로 사용하거나 더 높은 수준의 추상화를 설정할 수 있는 기본적인 오퍼레이션이 정의됨
* Template 클래스에 Operations 인터페이스가 구현됨
* 템플릿은 일종의 SPI(Service Provider Interface)(서비스 제공 인터페이스)
* 바로 사용할 수 있고 매우 유용하지만, 일반적으로는 매번 반복되는 단계를 거쳐야 함
* 일반적인 패턴의 데이터 액세스에서는 repository가 더 좋은 옵션
* repository가 템플릿을 기반으로 하기 때문에 추상화를 더 높이더라도 잃을 게 없음

## 저장소 지원

* 스프링 데이터가 Repository 인터페이스를 정의하고, 이 인터페이스로부터 모든 유형의 스프링 데이터 repository 인터페이스가 파생됨
* 예로, JPARepository에서 몽고 DB 기능을 더 사용할 수 있는 MongoRepository 가 파생, CrudRepository에서 용도가 더 다양한 ReactiveCrudRepository, PagingAndSortingRepository 등이 파생
* 또한 다양한 repository 인터페이스는 findAll(), findById(), count(), delete(), deleteAll() 등과 같은 유용한 상위 수준 함수를 지정

## 레디스

* Redis는 일반적으로 서비스 내 인스턴스 간에 상태를 공유하고, 캐싱과 서비스 간 메시지를 중개하기 위해 인메모리 repository로 사용하는 데이터베이스

## 롬복

* @NoArgsConstructor -> 매개변수가 없는 생성자를 만들도록 지시
* @AllArgsConstructor -> 각 멤버 변수의 매개변수가 있는 생성자를 만들도록 지시하고,  모든 멤버 변수에 인수를 제공
* @Id -> 어노테이션이 달린 멤버 변수가 데이터베이스 항목/레코드의 고유 식별자를 가지도록 지정
* @JsonProperty("vert\_rate") -> 한 멤버 변수를 다른 이름이 붙은 JSON 필드와 연결
* @Data -> 모든 멤버 변수의 게터와 세터 메서드를 생성
* @JsonIgnoreProperties(ignoreUnknown=true) -> JSON 응답 필드 중에서 클래스에 상응하는 멤버 변수가 없는 경우, Jackson 역직렬화 매커니즘이 이를 무시하도록 함

## 템플릿 지원 추가하기

* 스프링 부트는 자동 설정으로 기본적인 RedisTemplate 기능을 제공하며, 만약 Redis로 String 값만 조작한다면 별도로 수행할 작업이나 코드가 거의 없음
* RedisTemplate 클래스는 RedisAccessor 클래스를 상속해 RedisOperations 인터페이스를 구현

```java
// 1. 객체와 JSON 레코드 간 변환 시 사용할 Serializer를 생성
// Jackson은 JSON 값의 마샬링/언마샬링(직렬화/역직렬화)에 사용
// 스프링 부트 웹 애플리케이션에 Jackson이 있으므로 Aircraft 타입의 객체를 위해 Jackson2JsonRedisSerializer 생성
// 2. String 타입 키와 Aircraft 타입의 값을 허용하는 RedisTemplate을 만듬
// 이 빈 생성 메서드의 유일한 매개변수인 RedisConnectionFactory factory 객체에 자동 주입된 Redis ConnectionFactory 빈을
// template 객체에 담아서 레디스 데이터 베이스에 커넥션을 생성하고 조회할 수 있게 함
// 3. 기본 serializer로 사용하기 위해 template 객체에 serializer를 제공
// 4. 기본 serializer는 Aircraft 타입의 객체를 기대하기 때문에, String 타입의 키를 변환하기 위해 다른 serializer를 지정
// Serializer에 StringRedisSerializer를 담아줌
// 마지막으로 애플리케이션 내에서 RedisOperations 빈의 구현체가 요청될 때 사용할 빈으로 RedisTemplate 타입의 template 객체를 반환

@Bean
public RedisOperations<String, Aircraft> redisOperations(RedisConnectionFactory factory) {
 Jackson2JsonRedisSerializer<Aircraft> serializer = new Jackson2JsonRedisSerializer<>(Aircraft.class);

 RedisTemplate<String, Aircraft> template = new RedisTemplate<>();
 template.setConnectionFactory(factory);
 template.setDefaultSerializer(serializer);
 template.setKeySerializer(new StringRedisSerializer());

 return template;
}
```

## 데이터 로드하기

* 데이터베이스를 초기화하고 채우는 가장 유용한 매커니즘 두가지
* DDL, DML 스크립트를 사용해 초기화하고 채우기
* 하이버네이트를 통해 @Entity 클래스에서 테이블 구조를 자동으로 생성하도록 하고 repository 빈을 통해 채우기
* @PostConstruct 사용
