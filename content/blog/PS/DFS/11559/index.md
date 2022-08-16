---
title: '백준 11559번 JAVA : Puyo Puyo'
date: 2022-08-17 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/11559

# 풀이전략

재미있는 뿌요뿌요 문제였다. 나는 DFS로 문제를 해결했지만, 결국은 구현 문제인거 같다. 이 문제에서 관건은 다음과 같다.

1. dy, dx 배열을 통해 상하좌우 인접 블록을 확인한다.
2. DFS를 통해 인접 블록 중, 찾으려는 블록과 같은 블록의 갯수를 찾는다. 
3. 블록의 갯수가 4개 이상이라면, 해당 블록들은 없애주고, map을 다시 update 한다. 
4. 한 턴에 여러개의 블록 종류가 터질 수 있다. 
5. 블록이 하나도 안 터질 수가 있다. 



# 코드

```java


package backjoon.P11559;

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static int H = 12;
    static int W = 6;
    static char[][] map;
    static boolean[][] visited;
    static int[] dy = {-1, 0, 1, 0};
    static int[] dx = {0, 1, 0, -1};
    static int sameCnt = 0;
    public static void DFS(int r, int c, char val){

        for(int i=0; i<4; i++){
            int rr = r + dy[i];
            int cc = c + dx[i];
            if(rr >= H || rr < 0 || cc >= W || cc < 0) continue;
            if(!visited[rr][cc] && map[rr][cc] == val){
                visited[rr][cc] = true;
                sameCnt++;
                DFS(rr, cc, val);
            }
        }
    }

    public static void markX(int r, int c, char val){
        for(int i=0; i<4; i++){
            int rr = r + dy[i];
            int cc = c + dx[i];
            if(rr >= H || rr < 0 || cc >= W || cc < 0) continue;
            if(map[rr][cc] == val){
                map[rr][cc] = 'X';
                markX(rr, cc, val);
            }
        }
    }

    public static void moveRow(int row, int col){
        for(int i=row; i>=1; i--){
            map[i][col] = map[i-1][col];
        }
        map[0][col] = '.';
    }

    public static void moveMap(){
        for(int j=0; j<W; j++){
            int i = H-1;
            while(i >= 0){
                if(map[i][j] == 'X'){
                    moveRow(i,j);
                } else i--;
            }
        }
    }

    public static void print(){
        for(int i=0; i<H; i++){
            for(int j=0; j<W; j++){
                System.out.print(map[i][j]);
            }
            System.out.println("");
        }
        System.out.println("|||||||||||||||||||||");
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        map = new char[H][W];
        visited = new boolean[H][W];
        for(int i=0; i<H; i++){
            String line = br.readLine();
            for(int j=0; j<line.length(); j++){
                map[i][j] = line.charAt(j);
            }
        }
        boolean flag = true;
        int answer = -1;
        while(flag){
            flag = false;
            answer++;
            for(int i=0; i<H; i++){
                for(int j=0; j<W; j++){
                    if(map[i][j] =='.' || visited[i][j]) continue;
                    visited[i][j] = true;
                    sameCnt = 1;
                    DFS(i, j, map[i][j]);
                    if(sameCnt >= 4){
                        flag = true;
                        markX(i,j,map[i][j]);
                    }
                }
            }
            for(int i=0; i<H; i++){
                for(int j=0; j<W; j++){
                    visited[i][j] = false;
                }
            }
            moveMap();

        }
        System.out.println(answer);



        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

그동안 다른 구현문제에 많이 길들여져서, 이번 문제는 나름 쉽게 해결할 수 있었다.