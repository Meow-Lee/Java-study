---
description: About Array
---

# Array

### 1. 배열

* 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것

### 2. 배열의 복사

* for문을 이용한 방법
* System.arraycopy()

```java
// for문 사용
for(int i=0; i<num.length; i++){
    newNum[i] = num[i];
    }
    
// System.arraycopy() 사용
System.arraycopy(num, 0, newNum, 0, num.length);

// num[0]에서 newNum[0]으로 num.length개의 데이터를 복사
```

* 배열의 복사는 System.arraycopy()를 사용하는 것이 효율적

### 3. String 클래스

* char 배열에 기능(메서드)을 추가한 것
* charAt(index) / substring(from, to) / equals(String) / toCharArray()

```java
// char array -> String
char[] chArr = {'A', 'B', 'C'}
String str = new String(chArr);

// String -> char Array
char[] tmp = str.toCharArray();

// print char array
Sytem.out.println(chArr);
```

### 4. 다차원 배
