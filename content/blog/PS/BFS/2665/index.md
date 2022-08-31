---
title: '백준 2665번 JAVA : 미로 만들기'
date: 2022-08-31 18:06:00
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2665

# 풀이전략

처음에는 완전탐색 문제인가 싶었지만, 결국 BFS로 해결할 수 있음을 알게되었다. 문제에서는 검은 방은 사면이 벽으로 둘러쌓여 있어서 뚫고 가야한다.

하지만 뚫고 가야한다는 것에 집중하면 안된다.

BFS로 진행하되, 흰 방일때는 그냥 지나가고, 검은 방일 때는 count 를 1 증가시키는 방법으로 count의 최솟값을 만들어가게끔 문제를 해결하면 된다. 마치 다익스트라 알고리즘과 비슷하다.

# 코드

```java
package backjoon.P2665;

import java.io.*;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    static int[] dy = {-1, 0, 1, 0};
    static int[] dx = {0, 1, 0, -1};

    static class Pos {
        int r, c, cnt;

        public Pos(int r, int c, int cnt) {
            this.r = r;
            this.c = c;
            this.cnt = cnt;
        }
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        int N = Integer.parseInt(br.readLine());

        int[][] map = new int[N][N];
        int[][] visited = new int[N][N];

        for (int i = 0; i < N; i++) {
            String line = br.readLine();

            for (int j = 0; j < line.length(); j++) {
                map[i][j] = line.charAt(j) - '0';
                visited[i][j] = Integer.MAX_VALUE;
            }
        }
        visited[0][0] = 0;
        Queue<Pos> q = new LinkedList<>();
        q.add(new Pos(0, 0, 0));

        while(!q.isEmpty()){
            Pos cur = q.poll();
            for(int i=0; i<4; i++){
                int rr = cur.r + dy[i];
                int cc = cur.c + dx[i];

                if(rr < 0 || cc < 0 || rr >=N || cc >=N){
                    continue;
                }
                // 하얀 벽일 때
                if(map[rr][cc] == 1){
                    int nextCnt = cur.cnt;
                    if(nextCnt >= visited[rr][cc]) continue;
                    visited[rr][cc] = nextCnt;
                    q.add(new Pos(rr, cc, nextCnt));
                }
                // 검은 벽일 때
                else {
                    int nextCnt = cur.cnt + 1;
                    if(nextCnt >= visited[rr][cc]) continue;
                    visited[rr][cc] = nextCnt;
                    q.add(new Pos(rr, cc, nextCnt));
                }
            }
        }

        System.out.println(visited[N-1][N-1]);

        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

문제를 보고 유형을 판단하는 것이 항상 어려운거 같다. 역시 꾸준히 풀어서 감을 유지하는 것이 중요하다.
