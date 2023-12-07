---
description: Chapter 4
---

# 데이터베이스 액세스

* 애플리케이션은 종종 여러 합당한 이유로 상태를 저장하지 않는 '무상태' API를 사용
* 하지만 완전히 무상태인 애플리케이션은 실제로 많지 않고 보통 특정 상태를 저장
* 예를 들면, 온라인 상점 장바구니로 보내는 요청에 상태를 담아 전송할 수 있지만, 일단 주문이 접수되면 장바구니 상태가 담긴 주문 데이터를 저장

## DB 액세스를 위한 자동 설정

* 스프링 부트는 개발자가 계속해서 반복적으로 수행하는 코드와 사용 패턴의 80\~90%를 최대한 단순화하는 것을 목표로 함
* 사용 패턴을 식별하면, 적절한 기본 구성을 사용해 필요한 빈을 자동으로 초기화

## 앞으로 얻게 될 것

* 이전에는 ArrayList로 커피 목록을 저장하고 관리
* 이 접근 방식은 간단하지만 단점이 존재
* 회복 탄력성이 떨어짐 -> 실행 중인 플랫폼에 장애가 발생하면 애플리케이션이 실행되는 동안 수행된 변경 내용이 모두 사라짐
* 애플리케이션 규모를 확장하기 어려움 -> 사용자가 많아져 확장하기 위해 추가로 인스턴스를 만들어 사용하면, 새로 생긴 인스턴스는 해당 인스턴스만의 고유한 커피목록을 가지게 됨
* 인스턴스 간에 데이터가 공유되지 않으므로, 변경 사항을 다른 인스턴스에 접속하는 사용자는 확인하지 못함
* 이를 해결하기 위한 여러 방법들을 다룰 것

## @Entity 사용하기

* 커피는 영속 가능 엔티티(Persistable Entity)이므로 Coffee 클래스에 어노테이션 추가

## Repository

* 스프링 데이터는 반복 작업을 해결하기 위해 저장소(Repository) 개념을 도입
* 스프링 데이터에 정의되었으며, 다양한 DB를 위한 추상화 인터페이스

## 초기 데이터 생성 기능을 별도의 컴포넌트로 분리하여 쉽게 활성화/비활성화

* @Component 클래스와 @PostConstruct 메서드를 사용하는 편이 좋음
* 애플리케이션 실행 시 CommandLineRunner, ApplicationRunner 등 여러 방법이 있지만, 이들이 repository 빈을 autowire하면, repository 빈을 Mock 객체로 대체하기 어려우므로 일부 단위 테스트가 제대로 동작하지 않음

```java
// 이제 DataLoader가 데이터 생성을 담당하고, RestApiDemoController는 API만을 제공
@Component
public class DataLoader {
    private final CoffeeRepository coffeeRepository;

    public DataLoader(CoffeeRepository coffeeRepository) {
        this.coffeeRepository = coffeeRepository;
    }

    @PostConstruct
    private void laodData() {
        coffeeRepository.saveAll(List.of(
                new Coffee("Cafe Cereza"),
                new Coffee("Cafe Ganador"),
                new Coffee("Cafe Lareno"),
                new Coffee("Cafe Tres Pontas")
        ));
    }
}
```
