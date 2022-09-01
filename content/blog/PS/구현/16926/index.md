---
title: '백준 16926번 JAVA : 배열 돌리기 1'
date: 2022-09-01 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/16926

# 풀이전략

배열을 돌리는 문제이다. 문제에는 특정한 규칙이 있다.

1. 가장 바깥쪽 사각형에서 안쪽으로 들어올 때마다 너비와 높이가 2씩 줄어든다.
2. 모든 회전을 전부 다 할 필요 없다. 사각형을 이루는 수만큼 Mod를 해주면 실질적으로 회전해야하는 회전수가 나온다.
3. 사각형을 이루는 갯수는 현재 사각형의 너비 - 다음 사각형의 너비이다. `w*h - (w-2)*(h-2)`

# 코드

```java


package backjoon.P16926;

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    static int N, M, R;
    static int[][] map;

    public static void rotateOne(int r, int c, int w, int h) {
        int tmp = map[r][c];
        // 윗줄 가로 옮김
        for (int i = c; i < c + w - 1; i++) {
            map[r][i] = map[r][i + 1];
        }
        // 오른쪽 세로 옮김
        for (int i = r; i < r + h - 1; i++) {
            map[i][c + w - 1] = map[i + 1][c + w - 1];
        }
        // 아랫줄 옮김
        for (int i = c + w - 1; i > c; i--) {
            map[r + h - 1][i] = map[r + h - 1][i - 1];
        }
        // 왼쪽 세로 옮김
        for (int i = r + h - 1; i > r; i--) {
            map[i][c] = map[i - 1][c];
        }
        map[r + 1][c] = tmp;
    }

    public static void rotate(int r, int c, int w, int h) {
        if (w == 0 || h == 0)
            return;

        int rotateMod = w * h - (w - 2) * (h - 2);
        int rotateCnt = R % rotateMod;

        for (int i = 0; i < rotateCnt; i++) {
            rotateOne(r, c, w, h);
        }
        rotate(r + 1, c + 1, w - 2, h - 2);
    }

    public static void print() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println("");
        }
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        R = Integer.parseInt(st.nextToken());

        map = new int[N][M];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        rotate(0, 0, M, N);
        print();
        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

규칙만 찾으면 금방 풀 수 있는 문제였다. 문제 풀이 전에 설계하는 것이 주요했다.
