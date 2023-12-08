---
description: Chapter 5
---

# 애플리케이션 설정과 검사

## @Value&#x20;

* 설정을 코드에 녹이는 가장 간단한 접근방식
* 패턴 매칭과 SpEL(Spring Expression Language)(스프링 표현언어)을 기반으로 구축되어 간단하고 강력함
* @Value 속성에도 제한이 있음
* greeting-coffee에는 기본값이 있어서 application.properties에서 해당 속성을 어노테이션 처리한다고 해도, GreetingController 내 @Value 어노테이션은 coffee 멤버 변수를 사용해 해당 속성값을 기본값으로 적절하게 처리
* 하지만 속성 파일에서 greeting-name, greeting-coffee를 모두 어노테이션 처리하면 속성값을 정의하는 환경 소스가 없어짐
* GreetingController 빈을 초기화할 때 가져오는 greeting-coffee 속성 안에는 어노테이션 처리를 하기 때문에 정의되지 않은 greeting-name이 있으므로 오류 발생

```java
// application.properties
greeting-name=Dakota
greeting-coffee=${greeting-name} is drinking Cafe Cereza

// 속성 기본값 사용
// 만약 application.properties에 있는 속성을 주석처리하면, @Value에 정의한 기본 값이 출력됨
@RestController
@RequestMapping("/greeting")
public class GreetingController {
    @Value("${greeting-name: Mirage}")
    private String name;

    @Value("${greeting-coffee : ${greeting-name} is drinking Cafe Ganador}")
    private String coffee;

    @GetMapping
    String getGreeting() {
        return name;
    }

    @GetMapping("/coffee")
    String getNameAndCoffee() {
        return coffee;
    }
}
```

## @ConfigurationProperties&#x20;

* @Value가 유연하지만 단점이 있기 때문에, @ConfigurationProperties를 만듬
* 이를 사용해 속성을 정의하고 관련 속성을 그룹화해서, 도구로 검증 가능하고 타입 세이프한 방식으로 속성을 참조하고 사용
* @Value 기반 속성과 다른 점은, 어노테이션이 달린 멤버 변수에 기본값을 지정할 수 없다는 것
* application.properties 파일이 보통 애플리케이션 기본값을 정의하는데 사용되므로, 기본값 지정 기능이 없어도 유용
* 즉, 기본 속성값을 적용하기에 더 나은 방법

```java
// @ConfigurationProperties
// SpringApplication에도 @ConfigurationPropertiesScan 붙여줘야 함
@ConfigurationProperties(prefix = "greeting")
public class Greeting {
    private String name;
    private String coffee;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCoffee() {
        return coffee;
    }

    public void setCoffee(String coffee) {
        this.coffee = coffee;
    }
}

@RestController
@RequestMapping("/greeting")
public class GreetingController {
    private final Greeting greeting;

    public GreetingController(Greeting greeting) {
        this.greeting = greeting;
    }

    @GetMapping
    String getGreeting() {
        return greeting.getName();
    }

    @GetMapping("/coffee")
    String getNameAndCoffee() {
        return greeting.getCoffee();
    }
}
```

## 잠재적 서드 파티 옵션

* 서드 파티 컴포넌트를 감싸고 해당 속성을 애플리케이션 환경에 통합하는 기능
* 직접 컴포넌트를 생성하는 대신 프로젝트에 외부 의존성을 추가하고, 컴포넌트 문서를 참조해 스프링 빈을 생성할 때 가장 유용

## 액추에이터

* 액추에이터(actuator)는 명사로 작동시키는 것, 엄밀히 말하면 무언가를 움직이게 하거나 제어하는 기계장치를 의미
*
