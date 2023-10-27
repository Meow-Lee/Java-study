---
description: About Variable
---

# Variable

### 1. 변수

* 단 하나의 값을 저장할 수 있는 메모리 공간
* 변수를 선언하면, 메모리의 빈 공간에 알맞은 크기의 저장 공간이 확보되고, 변수이름을 통해 사용할 수 있음

### 2. 기본형과 참조형

#### 기본형 (Primitive Type)

* 논리형 : boolean
* 문자형 : char
* 정수형 : byte, short, int, long
* 실수형 : float, double

#### 참조형 (Reference Type)

* 객체의 주소를 저장
* 8개의 기본형을 제외한 나머지 타입

### 3. 변수 vs 상수 vs 리터럴

* 변수 : 하나의 값을 저장하기 위한 공간
* 상수 : 값을 한 번만 저장할 수 있는 공간
* 리터럴 : 그 자체로 값을 의미하는 것

#### 상수가 필요한 이유

```java
final int WIDTH = 20;
final int HEIGHT = 10;

int triangleArea = (WIDTH * HEIGHT) / 2;
int rectangleArea = WIDTH * HEIGHT;
```

* 상수는 위와 같이 리터럴에 의미있는 이름을 붙여서 코드의 이해와 수정을 쉽게 만듬&#x20;
