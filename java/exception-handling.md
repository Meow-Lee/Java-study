---
description: About Exception Handling
---

# Exception Handling

### 1. 예외 처리

* 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것
* 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지할 수 있도록 하는 것
* 컴파일 에러 -> 컴파일 시 발생하는 에러
* 런타임 에러 -> 실행 시 발생하는 에러
* 논리적 에러 -> 실행은 되지만 의도와 다르게 동작하는 것

### 2. 에러 vs 예외

* 에러 -> 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
* 예외 -> 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류 / try-catch를 이용해 프로그램의 비정상 종료를 막을 수 있음

### 3. Exception

* IOException, ClassNotFoundExcepton, ... , RuntimeException
* Exception 클래스들 -> 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
* RuntimeException 클래스들 -> 프로그래머의 실수로 발생하는 예외

### 4. CheckedException VS UncheckedException

* CheckedException -> 실행 전 예측 가능한 예외, 반드시 예외 처리 (IOException, ClassNotFoundException)
* UnchekcedException -> 실행한 뒤에 알 수 있는 예외, 예외 처리의 여부를 선택할 수 있음

### 5. finally

* try-catch 문과 함께 예외의 발생 여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용
* try-catch-finally 순서
