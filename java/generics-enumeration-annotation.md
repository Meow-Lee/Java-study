---
description: About Generics, Enumeration, Annotation
---

# Generics, Enumeration, Annotation

### 1. Generics

* 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입 체크를 해주는 기능
* 타입 안정성 제공
* 타입 체크와 형변환을 생략할 수 있으므로 코드가 간결
* 즉, 다룰 객체의 타입을 미리 명시함으로서 번거로운 형변환을 줄여준다는 것

### 2. Generic 메서드

* 메서드의 선언부에 generic 타입이 선언된 메서드
* Collections.sort()

### 3. Enums

* 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 편리
* 상수의 값이 바뀌면, 해당 상수를 참조하는 모든 소스를 다시 컴파일해야 하지만, 열거형 상수를 사용하면 기존의 소스를 다시 컴파일하지 않아도 상관없음
* 열거형 상수 간의 비교에 '==' 를 사용할 수 있음 -> 그만큼 빠른 성능 제공
* 열거형 상수는 각각 객체이고, 객체의 주소를 가지고 있으며 변하지 않는 값이기 때문에 '==' 사용 가능
* 비교 연산자는 사용 불가능, compareTo() 로 사용

```java
// example
class Card {
    enum Kind {CLOVER, HEART, DIAMOND, SPADE}
    enum Value {TWO, THREE, FOUR}
    
    final Kind kind;
    final Value value;
}
```

### 4. Annotation

* 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함 시킨 것

#### Annotation 요소의 규칙

* 요소의 타입은 기본형, String, enum, Annotation, Class 만 허용
* () 안에 매개변수를 선언할 수 없음
* 예외 선언할 수 없음
* 요소를 타입 매개변수로 정의할 수 없음

```java
// annotation 정의
@interface 애너테이션이름 {
    타입 요소이름(); // 애너테이션 요소 선언
    ...
}

// example
@interface TestInfo {
    int count();
    String testedBy();
    ...
    TestType testType(); // enum TestType {FIRST, FINAL}
    DateTime testDate(); // 자신이 아닌 다른 애너테이션 포함 가능
}

@interface DateTime {
    String yymmdd();
    String hhmmss();
}
```
