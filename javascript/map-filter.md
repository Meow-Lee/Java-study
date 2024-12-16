# 객체 생략 표기법 / map, filter

* 객체의 속성명과 설정할 변수명이 같으면 생략 가능

```javascript
// 속성명과 변수명이 같은 경우 1
const name = "누시다";
const age = 24;

const user = {
    name: name,
    age: age
};

// 속성명과 변수명이 같은 경우 2
const user = {
    name,
    age
};
```



## map, filter

```javascript
// 배열.map()
const nameArr = ["누시다", "사키오카", "고토"];
const nameArr2 = nameArr.map((name) => return name);

// filter
const numArr = [1,2,3,4,5];
const newNumArr = numArr.filter((num) => {return num % 2 === 1; });

// index 다루기
nameArr.map((name, index) => console.log(`${index + 1} 번째는 ${name} 입니다.`));

// 사양 구현 예
const newNameArr = nameArr.map((name) => {
    if (name === "누시다") {
        return name;
    } else {
        return `${name}님`;
    }
});
```

