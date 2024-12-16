# 개요

## DOM이란(Document Object Model)

* HTML을 해석해서 트리 구조로 나타낸 것

## 가상 DOM

* 자바스크립트 객체로 만들어진 가상의 DOM
* 실제 DOM과 차이를 비교하고 **변경된 부분만을 실제 DOM에 반영**하여 DOM 조작을 최소화



## 패키지 관리자

* 자바스크립트는 npm 패키지 관리자를 사용

### npm 의 장점

* 의존 관계를 의식하지 않아도 자동으로 해결
* 팀 안에서 패키지를 공유하고 버전 통일이 쉬움
* 전 세계에 공개된 패키지를 하나의 명령어로 이용할 수 있음
* 어디에서 로딩한 것인지 알기 쉬움
* npm install -> package.json이 변경되고 패키지 정보 추가
* npm -> package-lock.json, yarn -> yarn.lock 이 생성되는데, lock 파일에는 패키지가 내부에서 사용하는 다른 패키지의 버전 정보나 의존 관계가 기록됨



## 모듈 핸들러와 트랜스파일러

* 모듈 핸들러는 빌드 시에 여러 파일을 하나로 모아줌
* 트랜스파일러는 자바스크립트 표기법을 브라우저에서 실행할 수 있는 형태로 변환시켜줌
* 공통적인 목적은, 높은 효율로 개발하고 실행 시 적절하게 변환하는 것



## SPA와 기존 웹 시스템의 차이

* 리액트를 비롯한 모던 자바스크립트 웹 시스템은 SPA(Single Page Application)로 작성됨
* 기본적으로 HTML 파일은 하나만 사용하고 자바스크립트를 이용해 화면을 전환함

### 기존 웹 시스템

* 페이지를 이동할 때마다 서버에 요청을 전송하고 서버 측에서 HTML을 반환받음
* 때문에 화면 전환 시 잠깐 하얗게 보이는 것이 특징

### SPA 웹 시스템

* 서버에 요청을 전송하고 HTML을 반환받는 것까지는 동일
* 다만, 페이지 표시에 필요한 데이터가 있을 시 데이터 요청을 전송하고 자바스크립트로 DOM을 바꿔 사용하여 변경된 부분만 업데이트하여 페이지 이동을 구현
* 또한, HTML 파일 요청과 달리 비동기적 실행을 통해 데이터를 얻음
* 따라서 화면이 하얗게 변하지 않음
* **HTML 파일이 하나이며 자바스크립트를 사용해 DOM을 바꿔 써서 화면 이동을 구현하는 것이 기본**