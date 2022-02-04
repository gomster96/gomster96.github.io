---
title: '벨만포드 알고리즘 (Bellman-Ford-Moore)'
date: 2022-01-27 14:59:19
category: 'Algorithm'
draft: false
---

<!-- <p align="center"><img src="1.png" height="600px" width="600px"></p> -->

# 벨만 포드 알고리즘이란?

벨만포드 알고리즘 이란 V개의 정점과 E개의 간선을 가진 그래프 G에서 특정 출발 정점(S)에서 부터 다른 `모든 정점까지의 최단 경로`를 구하는 알고리즘이다.

- V개의 정점과 E개의 간선을 가진 가중 그래프에서 어떤 정점 A에서 어떤 정점 B까지의 최단거리는 최대 V - 1 개의 간선을 사용한다.
- 최단거리를 구하기 위해 V - 1 번 E개의 모든 간선을 확인한다.
- 음의 가중치를 가지는 간선도 가능하다.
- 음의 사이클의 존재 여부를 확인할 수 있다.
  - 존재 여부 확인할 하기 위해 한번 더 E개의 간선을 확인한다.
  - V번쨰 모든 간선을 확인한 후 다시 간선을 확인할 때 거리 배열이 갱신되었다면 그래프는 음의 싸이클을 가지는 그래프이다.

# 벨만 포드 알고리즘 코드

```java
static class Main{
    class Edge{
        int from;
        int to;
        int dis;
        public Edge(int from, int to, int dis){
            this.from = from;
            this.to = to;
            this.dis = dis;
        }
    }

    public static void main(String[] args) throws Exception{

        // 정점의 개수는 N이라고 하고, 간선의 개수를 M개 있다고 할 때
        int[] distance = new int[N+1];
        Edge[] edgeList = new Edge [M+1];

        for(int i=1; i<=N; i++) distance[i] = Integer.MAX_VALUE;
        for(int i=1; i<=N; i++) edgeList[i]= new Edge("간선에 대한 정보 저장");

        int start = 1; // 일단 start를 1번정점이라고 가정함
        distance[start] = 0; // 자기 자신으로 가는 길이이니까 0으로 초기화해준다.

        // 사용할 수 있는 간선의 수를 N-1개까지 사용해보면서
        // 할 수있는 최단거리를 구하는 것이다.
        for(int i=1; i<=N-1; i++){
            // i는 간선을 사용하는 횟수이다. 즉 i가 1이면 간선을 한번만 사용해서 갈 수 있는 구간을 찾는다.
            // i가 1... n이면 간선을 3번 사용해서 갈 수있는 값이
            // 이전까지의 값(1,2 .... n-1번 사용)보다 작으면 최신화 시켜준다.

            // start가 1이고 distance[1] = 0 이기 때문에 당연히 1부터 시작하는
            // 다른 모든 정점으로의 최솟값으로 구해질 수밖에 없다.
            for(int j=1; j<=M; j++){
                Edge nowEdge = edgeList[j];
                if(distance[nowEdge.from] != Integer.MAX_VALUE){
                    if(distance[nowEdge.to] > distance[nowEdge.from] + nowEdge.distance){
                        distance[nowEdge.to] = distance[nowEdge.from] + nowEdge.distance;
                    }
                }
            }
        }

        boolean findNegativeCycle = false;
        // 각 간선을 확인하며 지금까지 구했던 값들보다(N-1개의 간선을 사용하는 방법)
        // 더 작은 값이 발생한다면 음의 싸이클이 존재한다는 뜻이다.
        for(int j=1; j<=M; j++){
            Edge nowEdge = edgeList[j];
            if(distance[nowEdge.from] != INF){
                if(distance[nowEdge.to] > distance[nowEdge.from] + nowEdge.distance){
                    findNegativeCycle = true;
                    break;
                }
            }
        }
        // 음의 싸이클이 발생하면 distance배열의 값들이 의미가 없어진다.
        // 만약 findNegativeCycle이 false라면  distance배열에는 각 값들중 최단거리가 저장되어있다.
    }
}

```

벨만포드 알고리즘은 최단거리를 구하기위해 총 E개의 모든 간선을 V-1번 확인하므로 시간복잡도는 O(VE)이다. (이때 E는 간선의 개수, V는 정점의 개수)

# Reference
