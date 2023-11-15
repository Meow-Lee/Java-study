---
description: 타임리프 기본 기능
---

# 기본 기능

### 1. 타임리프 특징

* 서버 사이드 HTML 렌더링 (SSR) (Server Side Rendering)
* Natural Template (순수 HTML을 최대한 유지하면서 뷰 템플릿도 사용할 수 있는 특징)
* 스프링 통합 지원

### 2. 사용 선언과 기본 표현식

```html
// 사용 선언
<html xmlns:th="http://www.thymeleaf.org">

// 기본 표현식
${...} // 변수 표현식
@{...} // 링크 표현식
#{...} // 메시지 표현식
*{...} // 선택 변수 표현식
~{...} // 조각 표현식

// 문자 연산
|The name is ${...}| // 리터럴 대체


// 텍스트 출력
<span th:text="${data}">

// SpringEL
model.addAttribute("user", userA); // 객체
model.addAttribute("users", list); // 리스트
model.addAttribute("userMap", map); // 맵

// 컨트롤러에서 모델에 위와 같이 넘겼을 경우, 아래와 같이 프로퍼티에 접근할 수 있음
user.username
users[0].username
userMap['userA'].username

// 지역변수
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text=${first.username}"></span></p>
</div>
```

### 3. 유틸리티 객체

```html
// #temporals
model.addAttribute("localDateTime", LocalDateTime.now());
<span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"

#message ...
```

### 4. URL 링크

```html
// model.addAttribute("param1", "data1") , model.addAttribute("param2", "data2")
// 단순 URL
@{/hello} -> /hello

// 쿼리 파라미터 ()부분은 쿼리로 처리
@{/hello(param1=${param1} param2=${param2}}} -> /hello?param1=data1&param2=data2

// 경로 변수 변수가 있으면 ()부분은 경로변수로 처리
@{/hello/{param1}/{param2}(param1=${param1} param2=${param2})} -> /hello/data1/data2
```

### 5. 반복, 조건

```markup
// th:each
List<User> list = new ArrayList<>();
model.addAttribute("users", list);

<tr th:each="user:${users}">
    <td th:text=${user.username}>username</td>
    <td th:text=${user.age}>0</td>
</tr>

or

<tr th:each="user, userStat : ${users}>
    ...
</tr>

// th:if, th:unless
<span th:if="${user.age lt 20}">
<span th:unless=${user.age ge 20}">

// th:switch, th:case
<td th:switch="${user.age}">
    <tr th:case="10">10살</tr>
    ...
    <tr th:case="*">기타</tr> 
</td>
```

### 6. 템플릿 조각

* 웹 페이지 개발 시 공통 영역들이 존재하는데 이를 효율적으로 개발하기 위해 제공하는 기능

```markup
// 템플릿 조각
<footer th:fragment="copy">
  푸터 자리 입니다.
</footer>

// 현재 태그(div)에 추가
<h2>부분 포함 insert</h2>
<div th:insert="~{template/fragment/footer :: copy}"></div>

// 현재 태그(div)를 대체
<h2>부분 포함 replace</h2>
<div th:replace="~{template/fragment/footer :: copy}"></div>
```
