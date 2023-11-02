---
description: About classes
---

# Many Classes

### 1. hashCode()

* 해싱 기법에 사용되는 해시 함수를 구현한 것
* 해시 함수는 찾고자 하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드를 반환

### 2. new String() VS 리터럴 ("")

* new String() -> new 키워드로 새로운 객체를 생성하기 때문에 Heap 메모리 영역에 저장
* "" -> Heap 안에 있는 String Constant Pool 영역에 저장

### 3. String VS StringBuffer VS StringBuilder

* String -> 불변의 속성
* StringBuffer -> 가변의 속성 / 동기화 지원하여 멀티 스레드 환경에서 주로 사용
* StringBuilder -> 가변의 속성 / 동기화 지원하지 않아 싱글 스레드 환경에서 주로 사용

### 4. Wrapper class

* 기본 자료형에 대한 객체 표현 (기본  타입의 데이터를 객체로 표현해야 할 때 객체로 다루기 위해 사용하는 클래스)

### 5. Autoboxing, Unboxing

* autoboxing -> 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것
* unboxing -> 반대로 변환하는 것 (기본 자료형으로 변환)

### 6. Reflection (리플렉션)

* 구체적인 클래스 타입을 알지 못해도, 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API
* 코드 작성 시점에서 어떤 타입의 클래스를 사용할 지 모르지만, 런타임 시점에 지금 실행되고 있는 클래스를 가져와서 실행해야 하는 경우에 사용

### 7. Optional API

* NPE(NullPointerException) 을 피하려면 Null 여부 검사는 필연적
* Optional를 활용해 Null로 인한 예외를 피하고, Optional 클래스의 메소드를 통해 Null을 컨트롤
