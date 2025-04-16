## [Algorithm] 오늘 푼 문제

### [서강그라운드](https://www.acmicpc.net/problem/14938) 

모든 노드 간의 최단 경로를 구해야 한다. 그리고 노드가 최대 100개이므로 시간 복잡도는 $O(V^3)$인 플로이드-워셜이 출제의도라고 볼 수 있다. 하지만 주어진 그래프에서 음수 가중치는 주어지지 않으므로 다익스트라로 모든 노드에 대해서 진행하면 동일하게 답을 구할수 도 있다.

#### 플로이드-워셜

최단 거리 배열을 인접 행렬로 구현한다. 자기 자신은 0, 나머지는 INF로 초기화 한다.

```java
int[][] dist = new int[n + 1][n + 1];
for (int i = 1; i <= n; i++) {
    Arrays.fill(dist[i], INF);  //나머지는 INF (도달 불가)
    dist[i][i] = 0;  //자기 자신은 0
}
```

그리고 점화식으로 거리 배열을 업데이트 한다.

```java
//플로이드-워셜 -> 노드 수는 100, O(100^3)으로 해결 가능
for (int k = 1; k <= n; k++) {
    for (int s = 1; s <= n; s++) {
        for (int e = 1; e <= n; e++) {
            int newDist = dist[s][k] + dist[k][e];
            if (dist[s][e] > newDist) {
                dist[s][e] = newDist;
            }
        }
    }
}
```

#### 다익스트라

다익스트라는 우선순위 큐를 이용해서 구현한다. 다음과 같이 `Edge` 클래스로 간선 정보를 저장한다.

```java
private static class Edge {
    int to, cost;

    public Edge(int to, int cost) {
        this.to = to;
        this.cost = cost;
    }
}
```

모든 노드에 대해서 다익스트라를 진행하며 간선의 경로가 짧은 순으로 큐를 정렬하고, 최단 거리일 때만 거리 배열을 갱신한다.

```java
private static void dijkstra(int start) {
    dist = new int[n + 1];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    PriorityQueue<Edge> pq = new PriorityQueue<>(Comparator.comparingInt(e -> e.cost));
    pq.add(new Edge(start, 0));

    while (!pq.isEmpty()) {
        Edge cur = pq.poll();
        for (Edge next : adjList.get(cur.to)) {
            int newDist = cur.cost + next.cost;
            if (dist[next.to] > newDist) {  //최단 거리일 때만 갱신
                dist[next.to] = newDist;
                pq.add(new Edge(next.to, dist[next.to]));
            }
        }
    }
}
```
***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)