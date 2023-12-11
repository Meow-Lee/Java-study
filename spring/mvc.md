---
description: Chapter 7
---

# 스프링 MVC로 만드는 애플리케이션

## 스프링 MVC는 무엇을 의미할까

* 스프링 애플리케이션에서 모델-뷰-컨트롤러 패턴의 구현
* Model 인터페이스, @Controller 클래스, 뷰 기술처럼 스프링 MVC 컴포넌트 개념을 사용한 애플리케이션의 생성
* 스프링을 사용한 블로킹/논-리액티브 애플리케이션의 개발

## 템플릿 엔진

* 사용자의 브라우저에서 실행되고 표시될 최종 페이지를 생성하는, 서버 사이드 애플리케이션을 위한 방법을 제공
* 요청된 리소스를 수행할 뷰/템플릿을 결정하는 뷰 리졸버 제공
* 예상 결과를 생성하기 위해 템플릿 엔진이 사용할 입력을 정의하는 템플릿 언어 및 태그 모음
* Thymeleaf는 가장 널리 사용되며, 스프링 MVC와 스프링 웹플럭스 애플리케이션을 모두 지원
* 일반적인 템플릿을 사용하고, 파일에는 코드 요소가 통합되었으며, 표준 웹 브라우저에서도 직접 코드를 볼 수 있음