# 템플릿 문자열과 화살표 함수

## 템플릿 문자열

* 문자열 안에서 변수를 전개하기 위한 새로운 표기법

```javascript
// 기존 코드
const name = "누시다";
const age = 24;

const msg = "내 이름은" + name + "입니다. 나이는 " + age + "세입니다.";

// 변경된 코드
const name = "누시다";
const age = 24;

const msg = `내 이름은 ${name} 입니다. 나이는 ${age} 세입니다.";
```

* 템플릿 문자열을 이용하는 경우 \` 을 사용
* ${} 를 사용하여 변수를 간단하게 전개할 수 있음



## 화살표 함수 () ⇒ {}

```
// 기존
function func1(value) {
    return value;
}

const func1 = function(value) {
    return value;
};

// 화살표 함수
const func2 = (value) => {
    return value;
};

// 인수가 한 개인 경우
const func2 = value => {
    return value;
};

// 두 개 이상인 경우
const func3 = (value1, value2) => {
    return value;
};

// 처리를 한 행으로 반환하는 경우
const func4 = (num1, num2) => num1 + num2;

// 반환값이 여러 행인 경우
const func5 = (val1, val2) => (
    {
        name: val1,
        age: val2
    }
)

```

### 주의점

* 인수가 한 개인 경우 소괄호 생략이 가능
* 두 개 이상인 경우는 생략 불가
* 처리를 한 행으로 반환하는 경우 중괄호와 return을 생략할 수 있음
