---
description: 타임리프 스프링 통합과 폼
---

# 스프링 통합과 폼

### 1. 스프링 통합으로 추가되는 기능들

* 스프링의 SpringEL 문법 통합 -> ${@myBean.doSomething()} 처럼 스프링 빈 호출 지원
* 편리한 폼 관리를 위한 추가 속성 ( th:object, th:field, th:errors, th:errorclass )
* 폼 컴포넌트 기능
* 스프링의 메시지, 국제화 기능의 편리한 통합
* 스프링의 검증, 오류 처리 통합
* 스프링의 변환 서비스 통합(ConversionService)

### 2. 입력 폼 처리

```markup
// th:object 커맨드 객체를 선택
model.addAttribute("iteM", new Item());

// th:object를 사용해 <form>에서 사ㅇ용할 객체를 선택
// th:field와 선택변수 표현식인 *{...}를 사용하면 자동으로 처리해줌
// th:field="*{itemName}" == "${item.itemName}"
<form action="item.html" th:action th:object="${item}" method="post">
    <div>
        <label for="itemName">상품명</label>
        <input type="text" id="itemName" th:field="*{itemName}" class="form-control" placeholder="이름을 입력하세요">
    </div>
    <div>
        <label for="price">가격</label>
        <input type="text" id="price" th:field="*{price}" class="form-control" placeholder="가격을 입력하세요">
    </div>
    <div>
        <label for="quantity">수량</label>
        <input type="text" id="quantity" th:field="*{quantity}" class="form-control" placeholder="수량을 입력하세요">
    </div>
</form>
```
