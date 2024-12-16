# 분할 대입 {} \[] / 디폴트값 = / 스프레드 구문

## 분할 대입

* 객체나 배열로부터 값을 추출하기 위한 방법

```javascript
// 기존 방법
const myProfile = {
    name: "누시다",
    age: 24
};

const msg = `내 이름 ${myProfile.name}, 나이 ${myProfile.age}`;

// 객체 분할 대입
const myProfile = {
    name: "누시다",
    age: 24
};

const {name, age} = myProfile;
const msg = `내 이름 ${name}, 나이 ${age}`;

// 부분 추출도 가능, 순서도 상관없음, 별명 지정도 가능
const {age} = myProfile;

const {age, name} = myProfile;

const {name: newName, age: newAge} = myProfile;
const msg = `내 이름 ${newName}, 나이 ${newAge}`;
```

```javascript
// 배열 분할 대입

// 배열 인덱스를 지정해서 대입
const myProfile = ["누시다", 24];

const msg = `내 이름 ${myProfile[0]}, 나이 ${myProfile[1]}`;

// 배열에 분할 대입
const [name, age] = myProfile;
const msg = `내 이름 ${name}, 나이 ${age}`;
```



## 디폴트 값

* 함수의 인수나 객체 분할 대입 경우 설정해 사용함
* 값이 존재하지 않을 때 초기값을 설정할 수 있어 안전하게 처리 가능

```javascript
// 객체 분할 대입의 디폴트 값
const myProfile = {
    age: 24
}

const {name = "게스트"} = myProfile;
const msg = `${name}님, 안녕하세요!`;
```



## 스프레드 구문 ...

### 요소 전개

```javascript
// 내부 요소를 순차적으로 전개
const arr1 = [1, 2];

console.log(arr1) // [1, 2]
console.log(...arr1) // 1 2

// 일반 함수와 스프레드 구문 비교
 const arr1 = [1, 2];
 
 const summaryFunc = (num1, num2) => console.log(num1 + num2);
 
 summaryFunc(arr1[0], arr1[1]); // 3
 summaryFunc(...arr1) // // 3
```

### 요소 모으기

```javascript
// 요소 모으기
const arr2 = [1, 2, 3, 4, 5];

const [num1, num2, ...arr3] = arr2;
console.log(num1); // 1
console.log(num2); // 2
console.log(arr2); // [3, 4, 5]
```

## 요소 복사와 결합

<pre class="language-javascript"><code class="lang-javascript"><strong>const arr4 = [10, 20];
</strong>const arr5 = [30, 40];

// 스프레드 구문을 이용해 배열 복사
const arr6 = [...arr4];

// 두 개의 배열 결합
const arr7 = [...arr4, ...arr5];

// 여러 객체 결합
const obj4 = {val1: 10, val2: 20};
const obj5 = {val3: 30, val4: 40};

const obj6 = {...obj4};
const obj7 = {...obj4, ...obj5};
</code></pre>

### 등호를 이용해서 복사하면 안되는 이유

* 배열이나 객체 등 오브젝트 타입인 변수는 등호로 복사하면 참조값 역시 상속되기 때문

```javascript
// =로 복사한 경우
const arr4 = [10, 20];

const arr5 = arr4;

arr5[0] = 100;

console.log(arr4); // [100, 20]
console.log(arr5); // [100, 20]

// 스프레드 구문으로 복사한 경우
const arr4 = [10, 20];

const arr5 = [...arr4];

arr5[0] = 100;

console.log(arr4); // [10, 20]
console.log(arr5); // [100, 20]
```

