---
title: '백준 3055번 JAVA : 탈출'
date: 2022-01-18 18:06:00
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/3055

# 풀이전략

고슴도치가 물을 피해 비버 굴로 가는 문제이다. 최소시간 즉 최단거리를 구하는 문제이므로 BFS로 풀었다.

1. 고슴도치와 물은 상하좌우를 움직일 수 있다. -> dy, dx 배열을 만들어서 움직임을 구현한다.
2. 고슴도치는 물이 찰 예정인 칸으로 이동할 수 없다
   - 큐에 고슴도치를 제일 마지막에 넣어주면, 물이 먼저 이동한 후 고슴도치가 이동하는 것을 구현할 수 있다.
3. 물은 .과 S인 지역만, 고슴도치는 .과D 만 이동할 수 있다.
4. 비버의 굴로 이동할 수 없다면 "KAKTUS"를 출력한다.

# 코드

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.*;

public class B_3055 {
    static class Pair{
        int r, c;
        Pair(int r, int c){
            this.r = r;
            this.c = c;
        }
    }
    static int[] dy = {1, 0, -1, 0};
    static int[] dx = {0, 1, 0, -1};
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int R = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());
        char[][] board = new char[R][C];
        int[][] visited = new int[R][C];
        Pair gosum = new Pair(0,0);
        Queue<Pair> q = new LinkedList<>();
        for(int i=0; i<R; i++){
            String s = br.readLine();
            for(int j=0; j<C; j++){
                board[i][j] = s.charAt(j);
                if(board[i][j] == '*') q.add(new Pair(i,j));
                if(board[i][j] == 'S') gosum = new Pair(i,j);
            }
        }
        q.add(gosum);


        visited[gosum.r][gosum.c] = 1;
        while(!q.isEmpty()){

            Pair next = q.poll();
            int r = next.r;
            int c = next.c;

            for(int i=0; i<4; i++){
                int rr = r + dy[i];
                int cc = c + dx[i];
                // 영역 안에 있는지
                if(rr >= R || rr < 0 || cc >=C || cc < 0 ) continue;
                // 이동지점이 물이나 돌일 경우 이동 불가
                if(board[rr][cc] == '*' || board[rr][cc] == 'X') continue;

                // 고슴도치일때
                if(visited[r][c] != 0){

                    if(board[rr][cc] == 'D'){
                        System.out.println(visited[r][c]);
                        System.exit(0);
                    } else {
                        if(visited[rr][cc] == 0){

                            visited[rr][cc] = visited[r][c] + 1;
                            q.add(new Pair(rr,cc));
                        }
                    }
                }
                // 물일 때
                else if(board[r][c] == '*' && board[rr][cc] != 'D'){

                    board[rr][cc] = '*';
                    q.add(new Pair(rr,cc));
                }



            }
        }

        System.out.println("KAKTUS");
    }
}


```

# 회고

- 처음에 Pair를 선언할 때 사실 땅의 type을 하나 더 넣어줘도 좋았을 뻔 했는데, 무리하게 Pair로 구현하려고 해서 뭔가 더 햇갈렸던거 같다.
- 고슴도치는 물의 움직일 예정인 곳을 가지 못했기 때문에, 큐에 마지막으로 고슴도치의 위치를 넣어주는 아이디어가 주효했다.
- dy,dx배열을 만들어서 움직임을 구현해야한다.
