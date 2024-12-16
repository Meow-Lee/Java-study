# const, let 을 이용한 변수 선언

### var를 이용한 변수 선언의 문제점

* 변수를 덮어쓸 수 있고 재선언이 가능하다는 점에서 var은 거의 사용되지 않음

### let을 이용한 변수 선언

* let은 재선언을 할 수 없지만 덮어쓸 수 있음

### const를 이용한 변수 선언

* 재선언, 덮어쓰기가 모두 불가능한 선언 방법

### const로 정의한 변수를 변경할 수 있는 예시

* 문자열, 수치 등 primitive type이라 불리는 종류의 데이터는 덮어쓸 수 없음
* 하지만 객체, 배열 등 오브젝트 타입 데이터들은 도중에 값을 변경할 수 있음
* Object Type = 객체, 배열, 함수 등
* Primitive Type = Boolean, Number, BigInt, String, null, undefined, Symbol 등

## 리액트 개발에서 이용하는 변수 선언

* const를 가장 많이 이용
* 동적으로 바뀌는 값은 State라는 다른 개념으로 값을 관리
* 그래서 대부분 const를 이용하고 State로 관리하지 않으면서 처리 도중 값을 덮어써야 하는 경우만 let을 사용

