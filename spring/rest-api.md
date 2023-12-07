---
description: Chapter 3
---

# REST API

## API를 왜 사용하고 어떻게 사용하는가

* 모든 것을 한 곳에서 실행하는 모놀리식(Monolithic) 애플리케이션의 시대는 끝남
* 그럼에도 모놀리식 애플리케이션이 유효한 경우가 있고 그 외에는 마이크로서비스를 사용
* 마이크로서비스에서는 기능을 작고 응집력 있는 청크(chunk)로 분할해 디커플링을 함(decoupling)
* 따라서, 더 빠른 배포와 용이한 유지보수가 가능한 유연하고 튼튼한 시스템을 구축하게 됨
* 어떤 마이크로서비스 분산 시스템에서든 통신이 핵심

## REST가 무엇이며, 왜 중요한지

* API는 개발자가 작성한 사양/인터페이스&#x20;
* API를 통해 라이브러리, 다른 애플리케이션, 서비스 같은 다른 코드를 사용할 수 있음
* REST API에서 REST란, Representational State Transfer의 약어
* 예를 들어 애플리케이션 A가 애플리케이션 B와 통신할 때, A는 B에서 통신 시점의 현재 상태(state)를 가져옴
* A는 B에 요청할 때마다 관련 '상태'의 '표현'을 제공(representation)
* 이런 동작 방식은 생존가능성과 회복 탄력성을 향상시킴
* 이렇게 처리하는 이유는 통신 문제가 발생하거나 B가 충돌해 재시작되어도, A와 상호작용한 '현재 상태'를 잃어버리지 않기 때문
* A가 다시 요청해 두 애플리케이션이 중단된 곳에서 통신을 이어감
* 일반적인 개념으로 이는 무상태(stateless) 애플리케이션/서비스 라고 함
* 무상태란, 수신자가 이전 요청의 상태를 유지하지 않는 방식
* 각 서비스는 일련의 서비스 간 상호작용에서조차 서비스마다 자체적으로 '현재 상태'를 가지며, 다른 서비스가 자기 서비스의 '현재 상태'를 저장하리라 기대하지 않기 때문
* 정리하면, 웹 리소스에 대한 표현을 사용하여 상태를 전이시키는 통신의 표준화된 인터페이스

## API, HTTP 메서드 스타일

* REST API는 POST/GET/PUT/PATCH/DELETE 등 HTTP 메서드를 기반으로 함
* OPTIONS/HEAD도 사용
* 두 메서드는 요청/응답 쌍에서 사용할 수 있는 통신 옵션을 조회하고(OPTIONS), 응답 메시지에서 바디를 뺀 응답 헤더를 조회하는(HEAD) 것에 사용

## @RestController 개요

* 스프링 MVC는 뷰가 서버 렌더링된 웹페이지로 제공된다는 가정하에, 데이터와 데이터를 전송하는 부분과 데이터를 표현하는 부분을 분리해 생성
* @Controller는 @Component의 스테레오 타입
* 따라서, 애플리케이션 실행 시, 스프링 빈(애플리케이션 내 객체로서 IoC 컨테이너에 의해 생성되고 관리됨)이 @Controller 어노테이션이 붙은 클래스로부터 생성됨
* @Controller가 붙은 클래스는 Model 객체를 받는데, 이로 표현 계층에 모델 기반 데이터를 제공
* 또한 ViewResolver와 함께 작동해 애플리케이션이 뷰 기술에 의해 렌더링된 특정 뷰를 표시하게 함
* @ResponseBody를 클래스나 메서드에 추가해서 JSON이나 XML 같은 데이터 형식처럼 형식화된 응답을 반환하도록 Controller 클래스에 지시할 수도 있음(기본적으로 JSON 사용)
* @RestController는 @Controller와 @ResponseBody를 하나의 어노테이션으로 결합한 것으로 이를 사용해 REST API를 만들 수 있음

## @PathVariable&#x20;

```java
// 아래와 같이 사용/ 경로에 적혀있는 {id} 부분은 URI(Uniform Resource Identifier) 변수
// 해당 값은 @PathVariable 어노테이션이 달린 id 매개변수를 통해 getCoffeeById 메서드에 전달됨
@GetMapping("/coffees/{id})
Optional<Coffee> getCoffeeById(@PathVariable String id){
    for (Coffee c : coffees) {
            if (c.getId().equals(id)) {
                return Optional.of(c);
            }
    }
    return Optional.empty();
}
```

## HTTP 상태 코드

* GET에는 특정 상태 코드를 지정하지 않음
* POST와 DELETE 메서드에는 상태 코드 사용을 권장
* PUT 메서드는 상태 코드 사용 필수

```java
// 객체와 상태 코드를 같이 반환하는 ResponseEntity 사용
// 만약 커피가 존재하지 않으면 커피를 추가하고 201 상태코드(created) 반환
// 존재한다면 커피와 200 상태코드(OK) 반환
@PutMapping("/{id}")
ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {
    int coffeeIndex = -1;

    for (Coffee c : coffees) {
        if (c.getId().equals(id)) {
            coffeeIndex = coffees.indexOf(c);
            coffees.set(coffeeIndex, coffee);
        }
    }

    return (coffeeIndex == -1) ? new ResponseEntity<>(postCoffee(coffee), HttpStatus.CREATED) : new ResponseEntity<>(coffee, HttpStatus.OK);
}
```
