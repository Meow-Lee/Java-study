---
description: 우선순위 큐를 사용해 시간복잡도를 개선한 알고리즘
---

# Dijkstra

## 개념

* 그래프 최단 경로를 구하는 알고리즘
* 하나의 정점에서 출발해 다른 모든 정점으로의 최단 거리를 구함
* 음수 가중치는 해당이 되지 않음
* 우선순위 큐를 사용하여 O(mlogN)까지 낮출 수 있음

## 과정

* 방문하지 않은 정점 중에 가장 거리가 짧은 정점부터 방문
* 해당 정점을 거쳐서 갈 수 있는 거리가 이전 기록한 값보다 작으면 갱신
* 여기서 가장 거리가 짧은 정점부터 방문하기 위해 우선순위 큐 우선순위 기준을 거리로 잡음

## 코드

```java
// Dijkstra

// custom class
static class Pair implements Comparable<Pair> {
    int vertex, cost;
    
    Pair(int vertex, int cost){
        this.vertex = vertex;
        this.cost = cost;
    }
    
    // 거리가 짧은 곳부터 방문해야하기 때문에 cost 기준 오름차순 정렬
    @Override
    public int compareTo(Pair pair){
        return this.cost - pair.cost;
    }
}

// 그래프 선언
static ArrayList<ArrayList<Pair>> graph = new ArrayList<>();
public static void main(){
    // 만약 에지리스트가 int[][] edges 로 주여졌을때, 정점의 수는 n 일 때
    // 그래프 구성
    for(int i=0; i<=n; i++){
        graph.add(new ArrayList<>());
    }
    for(int[] edge : edges){
        int start_vertex = edge[0];
        int end_vertex = edge[1];
        
        // 여기서 가중치는 동일하다고 가정하여 1로 넣음
        graph.get(start_vertex).add(new Pair(end_vertex, 1);
        // 무방향 그래프라면 아래 코드도 같이 추가
        graph.get(end_vertex).add(new Pair(start_vertex, 1);
    }
    
    // 다익스트라
    dijkstra(n, 1);
}

staic void dijkstra(int n, int start){
    // 방문 체크 배열과 거리 배열 추가, 만약 거리 배열을 main에서 처리할 경우 static으로 선언
    boolean[] visited = new boolean[n+1];
    int[] dist = new int[n+1];
    
    // 거리 배열을 최대값으로 초기화해주고, 시작점은 0으로 초기화
    Arrays.fill(dist, (int)1e9);
    dist[start] = 0;
    
    // 우선순위 큐
    PriorityQueue<Pair> pq = new PriorityQueue<>();
    pq.offer(new Pair(start, 0));
    
    while(!pq.isEmpty()){
        Pair cur = pq.poll();
        
        // 현재 정점을 방문하지 않았으면 방문 처리        
        if(!visited[cur.vertex]){
            visited[cur.vertex] = true;
        }
        
        // 현재 정점에 연결된 정점들을 둘러보면서
        for(Pair next : graph.get(cur.vertex)){
            // 만약 시작점부터 next정점까지의 기록된 거리 값이 시작점부터 cur, 현재 정점까지의 거리 +
            // cur ~ next까지의 가중치를 더한 것보다 크면 거리 값을 갱신해주고 pq에 값을 넣어줌
            if(dist[next.vertex] > dist[cur.vertex] + next.cost){
                dist[next.vertex] = dist[cur.vertex] + next.cost;
                pq.offer(new Pair(next.vertex, dist[next.vertex]));
            }
        }
    }
}
```

## 레퍼런스

[https://velog.io/@suk13574/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Java%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BCDijkstra-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98](https://velog.io/@suk13574/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Java%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BCDijkstra-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
