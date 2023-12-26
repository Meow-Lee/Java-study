---
description: N의 약수를 구할 때
---

# 약수 구하기

## N의 약수를 구할 때

* 보통 1\~N까지 완전탐색하며 N의 약수의 개수를 구하는 것은 보통 TLE 확률이 굉장히 높음
* 따라서, 소인수분해 또는 제곱근을 활용한 방법을 통해 시간을 줄임

## 소인수분해

* 만약 N=24라면, 2^3 \* 3^1 이므로 약수의 개수는 (3+1) \* (1+1) = 8이 됨

```java
// 소인수 분해
private int getDivisors(int num){
    int count = 1;
    for(int i=2; i*i<=num; i++){
        int power = 0;
        while(n%i==0){
            n/=i;
            power++;
        }
        count *= (power+1);
    }
    
    if(n>1){
        count *= 2;
    }
    
    return count;
}
```

## 제곱근

* 만약 N=24라면, 제곱근은 4.xxx이므로, 1\~4를 탐색
* 1 \* 24, 2 \* 12, 3 \* 8, 4\* 6 으로 총 8개가 됨
* 예외조건으로, N이 16일 때 4\*4는 하나를 빼줘야함
* 따라서, N%i==0 이면서 N/i==i 이면 하나만 더해주는 것으로 함
* 보통 제곱근을 더 많이 사용함

```java
// using sqrt
private int getDivisors(int num){
    int count = 0;
    double root = Math.sqrt((double)num);
    for(int i=1; i<=root; i++){
        if(num % i == 0){
            if(num / i == i){
                count += 1; // 4*4=16과 같은 경우는 약수는 4 하나 뿐
            }
            else{
                count += 2; // 2*8=16 처럼 2,8 총 두개의 약수
            }
        }
    }
    return count;
}
```
