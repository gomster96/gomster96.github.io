---
title: '백준 17142번 JAVA : 연구소 3'
date: 2022-08-12 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/17142

# 풀이전략

상화좌우 움직이는 BFS 문제지만 바이러스가 활성, 비활성 바이러스로 나뉜다. 또한 벽은 움직일 수 없으며, 비활성 바이러스도 활성바이러스를 만나면 활성 바이러스로 바뀐다.

1. 바이러스는 상화좌우 움직인다. dy, dx 배열을 사용한다.
2. 바이러스의 위치를 지정해주기 위해 조합(combination)을 사용한다.
3. 기준이 될 map 과 BFS를 돌 virusMap 두가지 배열을 만들어 문제를 해결한다.
4. BFS가 진행할 때마다, 바이러스의 위치를 바꿔주고, virusMap을 초기화 해준다.
5. 활성바이러스의 경우 처음시작을 1로 해주고, 마지막에 출력할 때 답 - 1 을 해준다.
   - 활성바이러스의 start 도 0이여야 하지만 빈칸과 구별해주기 위함.

# 내가 틀렸던 포인트

1. 비활성 바이러스라도 바이러스이다. 즉 모든 칸을 활성바이러스로 채울 필요 없다.
   - 답을 체크할 때 원래 바이러스가 존재하는 위치라면 해당 virusMap = 1 로 만들어주어 체크한다. (비활성 바이러스의 시간까지 마치 빈칸처럼 처리할 필요가 없기 때문)
   - 이후 BFS가 진행할 때는 비활성 바이러스도 마치 빈칸인 것처럼 구현한다.
2. 시작부터 모든 칸이 바이러스로 채워질 경우도 존재한다.

# 코드

```java


package backjoon.P17142;

import java.io.*;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    static int answer = Integer.MAX_VALUE;
    static int N;
    static int M;
    static int[][] map;
    static int[][] virusMap;
    static ArrayList<Pos> viruses = new ArrayList<>();
    static Queue<Pos> q = new LinkedList<>();
    static int[] dy = {-1, 0, 1, 0};
    static int[] dx = {0, 1, 0, -1};

    static void bfs() {
        while (!q.isEmpty()) {
            Pos cur = q.poll();
            if (virusMap[cur.r][cur.c] != 0 && virusMap[cur.r][cur.c] < cur.cnt)
                continue;

            for (int i = 0; i < 4; i++) {
                int rr = cur.r + dy[i];
                int cc = cur.c + dx[i];

                if (rr < 0 || cc < 0 || rr >= N || cc >= N)
                    continue;
                // 벽이 아니고, 빈칸이거나 이전 바이러스보다 수가 작으면
                // 여기서 virusMap[rr][cc] cur.cnt+1 >= 을 했어야했는데 그냥 > 를 해서 문제 발생 -> 이러면 메모리초과나옴
                // init 할때 virus도 0으로 처리해야함
                if (virusMap[rr][cc] != -1 && (virusMap[rr][cc] == 0 || virusMap[rr][cc] > cur.cnt + 1)) {
                    virusMap[rr][cc] = cur.cnt + 1;
                    q.add(new Pos(rr, cc, cur.cnt + 1));
                }
            }
        }
        checkAnswer();
    }

    static void initMap() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                virusMap[i][j] = map[i][j];
                if(map[i][j] == 2) virusMap[i][j] = 0;
            }
        }
    }

    static void print() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                System.out.print(virusMap[i][j]+ " ");
            }
            System.out.println("");
        }
        System.out.println("fin");
    }

    static void combination(boolean[] visited, int start, int n, int r) {
        if (r == 0) {
            initMap();
            for (int i = 0; i < visited.length; i++) {

                if (visited[i]) {
                    virusMap[viruses.get(i).r][viruses.get(i).c] = 1;
                    q.add(viruses.get(i));
                }
            }
            bfs();
            return;
        }

        for (int i = start; i < n; i++) {
            visited[i] = true;
            combination(visited, i + 1, n, r - 1);
            visited[i] = false;
        }
    }

    static void checkAnswer() {
        int maxTime = -1;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (virusMap[i][j] == 0)
                    return;
                // 비활성 바이러스의 경우를 위한 처리
                if(map[i][j] == 2) virusMap[i][j] = 1;
                if (virusMap[i][j] > maxTime){
                    maxTime = virusMap[i][j];
                }

            }
        }
        if (maxTime > 0 && maxTime < answer)
            answer = maxTime;
    }


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

        st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        map = new int[N][N];
        virusMap = new int[N][N];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
                if (map[i][j] == 2) {
                    viruses.add(new Pos(i, j, 1));
                }
                if (map[i][j] == 1)
                    map[i][j] = -1;
            }
        }
        boolean[] visited = new boolean[viruses.size()];
        combination(visited, 0, viruses.size(), M);
        if (answer == Integer.MAX_VALUE)
            System.out.println(-1);
        else
            System.out.println(answer - 1);
        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

항상 기본 반례에 대해서 생각해야한다.

- 모든 칸이 바이러스로 찰 경우
- 비활성 바이러스와 활성바이러스의 구별에 대하여

문제를 분석할 때 제대로 분석하지 않아, 나는 이렇게 반례를 찾는것을 힘들어 하는 것 같다. 조금 더 연습하고, 더 노력해야겠다.
