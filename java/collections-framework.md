---
description: About Collections Freamework
---

# Collections Framework

### 1. Collections Framework

* 다수의 데이터를 쉽고 효과적으로 관리할 수 있는 표준화된 방법을 제공하는 클래스의 집합

<figure><img src="../.gitbook/assets/컬렉션 프레임워크.png" alt=""><figcaption><p>Collections Framework</p></figcaption></figure>

#### List

* 순서가 있는 데이터의 집합
* 데이터 중복 허용
* ArrayList, LinkedList, Stack, Vector

#### Set

* 순서를 유지하지 않는 데이터의 집합
* 데이터 중복 허용 X
* HashSet, TreeSet

#### Map

* key-value 의 쌍으로 이루어진 데이터의 집합
* 순서 유지 X, 키는 중복 허용 X, 값은 중복 허용
* HashMap, TreeMap, Hashtable, Properties

### 2. Iterator

* 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스
* boolean hasNext() -> 읽어 올 요소가 남아있는지 확인
* Object next() -> 다음 요소를 읽어옴
* void remove() -> next()로 읽어 온 요소를 삭제

```java
// Iterator
List list = new ArrayList();
Iterator it = list.iterator();

while(it.hasNext()){
    System.out.println(it.next());
}

// Map
Map map = new HashMap();
...
Iterator it = map.keySet().iterator();
Iterator it = map.entrySet().iterator();
```

### 3. Arrays

```java
// array -> list
List list = Arrays.asList(new Integer[]{1,2,3,4,5});
List list = Arrays.asList(1,2,3,4,5);
```

### 4. Comparator, Comparable

* Comparator -> 기본 정렬 기준 외 다른 기준으로 정렬할 때 사용
* Comparable -> 기본 정렬 기준을 구현하는데 사용
* compareTo() -> 두 객체가 같으면 0, 비교하는 값보다 작으면 음수, 크면 양수 반환

### 5. 정리

#### ArrayList

* 배열 기반
* 데이터 추가 및 삭제에 불리
* 순차적인 추가 삭제는 가장 빠름
* 임의의 요소에 대한 접근성이 뛰어남

#### LinkedList

* 연결 기반
* 데이터 추가 및 삭제에 유리
* 임의의 요소에 대한 접근성이 좋지 않음

#### HashMap

* key-value 형태
* 추가, 삭제, 검색, 접근성이 모두 뛰어남
* 검색에 최고의 성능

#### TreeMap

* 연결 기반
* 정렬과 검색 (특히 범위검색) 에 적합
* 검색 성능은 HashMap보다 떨어짐

#### Stack

* Vector를 상속 받아 구현

#### Queue

* LinkedList가 Queue 인터페이스를 구현

#### Properties

* Hashtable을 상속 받아 구현

#### HashSet

* HashMap을 이용해 구현

#### TreeSet

* TreeMap을 이용해 구현
