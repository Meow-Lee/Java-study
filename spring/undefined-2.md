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
* HTTP 엔드포인트나 JMX로 실행 중인 앱의 모니터링과 관리 기능을 제공하며 스프링 부트의 실제 제품 단계 수준 기능을 모두 포함하고 보여줌
* 액추에이터는 실행 중인 애플리케이션의 정보에 접근하고 이를 노출
* 이 정보는 개발자, 운영자 또는 악의적인 사람에게도 활용 가치가 매우 높음
* '기본적으로 안전하게'라는 스프링 시큐리티의 목표에 따라 액추에이터의 자동 설정은 매우 제한된 health 및 info 응답을 노출
* 행동이 기대와 일치하지 않는데 현재 애플리케이션 환경이나 상태를 완벽하게 알고있다고 가정하는 문제를 해결하는 것을 도와줌

````json
// localhost:8080/actuator
```json
{
    "_links": {
        "self": {
            "href": "http://localhost:8080/actuator",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8080/actuator/health/{*path}",
            "templated": true
        },
        "health": {
            "href": "http://localhost:8080/actuator/health",
            "templated": false
        }
    }
}
```

// application.properties에서 학습을 위해 와일드카드 문자만 사용해 보안을 완전히 비활성화
// management.endpoints.wbe.exposure.include=*
```json
{
    "_links": {
        "self": {
            "href": "http://localhost:8080/actuator",
            "templated": false
        },
        "beans": { // 애플리케이션에서 생성한 모든 스프링 빈
            "href": "http://localhost:8080/actuator/beans",
            "templated": false
        },
        "caches-cache": {
            "href": "http://localhost:8080/actuator/caches/{cache}",
            "templated": true
        },
        "caches": {
            "href": "http://localhost:8080/actuator/caches",
            "templated": false
        },
        "health": { // health 정보
            "href": "http://localhost:8080/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8080/actuator/health/{*path}",
            "templated": true
        },
        "info": {
            "href": "http://localhost:8080/actuator/info",
            "templated": false
        },
        "conditions": { // 스프링 빈의 생성 조건이 충족되었는지 여부
            "href": "http://localhost:8080/actuator/conditions",
            "templated": false
        },
        "configprops": { // 애플리케이션에서 액세스할 수 있는 모든 Environment 속성
            "href": "http://localhost:8080/actuator/configprops",
            "templated": false
        },
        "configprops-prefix": {
            "href": "http://localhost:8080/actuator/configprops/{prefix}",
            "templated": true
        },
        "env": { // 애플리케이션이 작동하는 환경의 무수한 측면 확인, 개별 configprop값의 출처확인에 유용
            "href": "http://localhost:8080/actuator/env",
            "templated": false
        },
        "env-toMatch": {
            "href": "http://localhost:8080/actuator/env/{toMatch}",
            "templated": true
        },
        "loggers": { // 모든 컴포넌트의 로깅 수준
            "href": "http://localhost:8080/actuator/loggers",
            "templated": false
        },
        "loggers-name": {
            "href": "http://localhost:8080/actuator/loggers/{name}",
            "templated": true
        },
        "heapdump": { // 트러블 슈팅과 분석을 위해 힙 덤프 시작
            "href": "http://localhost:8080/actuator/heapdump",
            "templated": false
        },
        "threaddump": { // 트러블 슈팅과 분석을 위해 스레드 덤프 시작
            "href": "http://localhost:8080/actuator/threaddump",
            "templated": false
        },
        "metrics": { // 애플리케이션에서 현재 캡처 중인 매트릭스
            "href": "http://localhost:8080/actuator/metrics",
            "templated": false
        },
        "metrics-requiredMetricName": {
            "href": "http://localhost:8080/actuator/metrics/{requiredMetricName}",
            "templated": true
        },
        "scheduledtasks": {
            "href": "http://localhost:8080/actuator/scheduledtasks",
            "templated": false
        },
        "mappings": { // 모든 엔드포인트 매핑과 세부 지원 정보
            "href": "http://localhost:8080/actuator/mappings",
            "templated": false
        }
    }
}
```
````

#### 액추에이터로 로깅 볼륨(수준) 높이기

* 소프트웨어를 개발하고 배포할 때와 마찬가지로, 프로덕션용 애플리케이션 로깅 수준을 선택할 때도 기회비용이 생김
* 액추에이터는 스프링 부트의 프로덕션용 단계 수준의 기능을 제공한다는 미션하에 이 문제까지 해결
* 개발자가 거의 모든 설정 요소에 "INFO" 같은 일반적인 로깅 수준을 설정하고, 중요한 문제 발생 시 애플리케이션에서 실시간으로 해당 로깅 수준을 일시적으로 변경하게 해줌
* 또한 해당 엔드포인트에 대한 간단한 POST로 로깅 수준을 설정/재설정하게 해줌
