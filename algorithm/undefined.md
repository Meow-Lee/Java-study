---
description: 정리
---

# 시간복잡도

* PriorityQueue -> 삽입, 삭제를 heap을 사용해야만 O(logN)으로 맞출 수 있음
* HashMap -> 삽입, 탐색, 삭제 등 모든 함수의 시간복잡도가 O(1) / 속도 빠르고 순서 정렬은 X
* TreeMap -> 균형이진트리 구조, 모든 함수의 시간복잡도가 O(logN) / Iterator를 사용해 순회하면, 자동으로 Key 값 기준 오름차순 순회
* HashSet -> 해싱 기반 자료구조, 모든 함수의 시간복잡도가 O(1) / 속도 빠르고, 순서 정렬은 X
* TreeSet -> 모든 함수의 시간복잡도가 O(logN) / 순서 정렬 됨
