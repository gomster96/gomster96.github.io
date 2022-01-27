---
title: '다익스트라 알고리즘 (Dijkstra Algorithm)'
date: 2022-01-27 10:59:19
category: 'Algorithm'
draft: false
---

<!-- <p align="center"><img src="1.png" height="600px" width="600px"></p> -->

# 다익스트라 알고리즘이란?

다익스트라 알고리즘은 V개의 정점과 `음수가 아닌` E개의 간선을 가진 그래프 G에서 특정 출발 정점(S)에서 부터 다른 `모든 정점까지의 최단 경로`를 구하는 알고리즘이다.

- 다익스트라 알고리즘은 음이 아닌 가중 그래프(간선의 weight가 있는 그래프)에서의 단일 쌍, 단일 출발, 단일 도착 최단 경로문제일때 푼다.
- 다익스트라 알고리즘은 BFS를 기본으로 한다.
- 음수 가중치가 포함되어 있다면 사용할 수 없다.
- Priority Queue 또는 heap을 사용하여 **O**(**E**log**V**)의 시간복잡도를 가진다.

# 자바 코드

```java
public class Main{

    static class Node{
        int v;
        int dis;
        public Node(int v, int dis) {
            this.v = v;
            this.dis = dis;
        }
    }
    public static void main(String[] args) throws Exception{
        // V 정점의 개수
        // E 간선의 개수일떄
        ArrayList<Node>[] graph = new ArrayList[V+1];
        // ArrayList의 배열로 각 정점에서 다른 정점으로 연결된 간선의 정보를 저장한다.


        int[] distance = new int[V+1];
        // 출발점에서 다른 정점으로 가는 최단거리를 저장하는 배열
        for(int i=0; i<V+1; i++){
            graph[i] = new ArrayList<>();
            distance[i] = Integer.MAX_VALUE;
        }

        // 이후 그래프 관련 정보들을 저장한다.

        // 현재 graph가 정리된 상태이며 start지점이 있다고 가정한다.
        // ex) graph[3].get(1) == 3번 정점에서 1번정점으로 가는 거리를 뜻한다.

        int start = 1;
        // 현재 start는 그냥 1이라고 칭하고 시작하겠다.
        PriorityQueue<Node> pq = new PriorityQueue<>((e1, e2) -> e1.dis - e2.dis);
        // 각 정점에서 다른 정점으로의 거리 즉 dis 의 오름차순으로 pq를 정렬한다.
        distance[start] = 0;
        // 시작점에서 자기 자신으로 가는 최단거리는 0이므로 이렇게 초기화해준다.
        pq.add(new Node(start, 0)); // 가장 먼저 시작할 start점 이므로 pq에 넣고 시작한다.

        while(!pq.isEmpty()){
            Node now = pq.poll();

            if(distance[now.v] < now.dis) continue;
            // 현재 now.v 즉 now의 정점으로 까지 가는 거리가
            // pq에서 저장된 정보인 now.v까지의 거리보다 짧을경우 변경할 사항이 없으므로 continue한다.
            // 이때 <= 는 하지 않는 이유는 거리가 같더라도 갈 수 있는 정점의 종류가 다를 경우도 있기 때문이다.


            // 현재 distance의 변경사항이 있거나, 정점의 종류가 다를수도 있으므로 해당 Node를 기반으로 다시
            // 갈 수 있는 graph를 확인한다.
            for(Node next : graph[now.v]){
                if(distance[next.v] > distance[now.v] + next.dis){
                    // 현재 distance배열에 저장된 next.v로 가는 최단거리가
                    // now.v를 거치고 now.v에서 next.v까지의 거리를 합한 것보다 큰 경우
                    // 최단거리를 업데이트 해준다.
                    distance[next.v] = distance[now.v] + next.dis;

                    pq.add(new Node(next.v, distance[next.v]));
                    // next.v로 가는 최단거리가 갱신되었기 때문에, 그 이후의 값들도 확인해야하기에
                    // pq에 값을 넣어준다.
                }
            }
        }
        // pq가 빌 때까지 계속하다보면 distance배열에 start에서 다른 정점으로 가는 최단거리가 저장되어있다.
    }
}
```

이 알고리즘의 시간복잡도가 **O**(**E**log**V**)인 이유는

1. pq에서 가장 짧은 dis의 Node를 찾기가 최대 O(logE)가 된다.
2. 우선순위 큐에 추가하는 간선의 개수는 최대 O(E) 만 큼 걸린다. 이때 E즉 간선의 개수는 V^2 즉 정점의 제곱보다 작다
3. O(ElogE) = O(ElogV^2) = O(2ElogV) = O(ElogV) 이다.

# Reference
