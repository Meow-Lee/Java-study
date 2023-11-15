---
description: About Union-Find
---

# Union-Find

```java
// Union-Find, 서로소 집합, Disjoint Set
import java.util.*;
import java.io.*;

public class UnionFind {
    static int[] parent; // 각 노드의 부모 노드를 설정하기 위한 배열
    public static void main(String[] args) {
        int n = 5;
        parent = new int[n + 1]; // 1번 ~ n번
        for (int i = 1; i <= n; i++) { // 초기값은 자기 자신
            parent[i] = i;
        }

    }

    // 부모 노드를 합치는 연산
    static void union(int x, int y) {
        x = find(x); // x의 부모 노드를 찾음
        y = find(y); // y의 부모 노드를 찾음

        if (x != y) { // 만약 x와 y의 부모 노드가 다르다면, y의 부모노드를 x의 부모노드로 지정하면서 합침
            parent[x] = y;
        }
    }

    // 부모 노드를 찾는 연산
    static int find(int x) {
        if (parent[x] == x) { // 만약 부모노드라면
            return x; // 값을 반환
        }
        return find(parent[x]); // 그렇지 않다면 부모노드를 재귀로 탐색
    }
}

```
