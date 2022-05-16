---
title: '백준 12865번 JAVA : 평범한 배낭'
date: 2022-05-16 16:23:02
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/12685

# 풀이전략

전형적인 DP문제로, 배낭문제이다. 문제에서 주어진 물건들을 넣는 횟수가 정해져있는지, 아닌지를 파악하며 문제를 해결해야한다.

전형적인 배낭문제로 풀이방법을 Top-Down, Bottom-Up 두 방법 다 익혀야한다.

- 나는 제대로 해결하지 못해 다른 분들의 코드를 참고했다.
- https://st-lab.tistory.com/141
- https://velog.io/@yanghl98/%EB%B0%B1%EC%A4%80-12865-%ED%8F%89%EB%B2%94%ED%95%9C-%EB%B0%B0%EB%82%AD-JAVA%EC%9E%90%EB%B0%94

# 코드

## TOP-DOWN

```java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static Integer[][] dp;
    static int[] W;
    static int[] V;

    static int solve(int i, int k){
        // i 가 0 미만일 때
        if(i < 0) return 0;

        if(dp[i][k] == null){

            // 현재 물건을 추가로 못담는 경우
            if(W[i] > k){
                dp[i][k] = solve(i-1,k);
            }
            // 현재 물건을 담을 수 있는 경우
            else {
                // 물건을 담기전의 값과 담은 값들을 모두 비교하여 더 큰값을 채택함
                dp[i][k] = Math.max(solve(i-1,k), solve(i-1, k-W[i]) + V[i]);
            }
        }
        return dp[i][k];
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        W = new int[N];
        V = new int[N];
        dp = new Integer[N][K+1];

        for(int i=0; i<N; i++){
            st = new StringTokenizer(br.readLine());
            W[i] = Integer.parseInt(st.nextToken());
            V[i] = Integer.parseInt(st.nextToken());
        }

        System.out.println(solve(N-1,K));
    }
}

```

## BOTTOM-UP

```java

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        int[][] item = new int[N+1][2]; //weight, value
        for(int i=1; i<=N; i++){
            st = new StringTokenizer(br.readLine());
            item[i][0] = Integer.parseInt(st.nextToken());
            item[i][1] = Integer.parseInt(st.nextToken());
        }

        int[][] dp = new int[N+1][K+1];

        for(int k=1; k<=K; k++){
            for(int i=1; i<=N; i++){
                dp[i][k] = dp[i-1][k];
                // k 값 즉 담을 수 있는 무게보다 item 작을 경우 배낭에 담을 수 있으므로 담는다.
                if(k - item[i][0] >= 0){
                    dp[i][k] = Math.max(dp[i-1][k], item[i][1] + dp[i-1][k-item[i][0]]);
                }
            }
        }
        System.out.println(dp[N][K]);

    }
}

```

## BOTTOM_UP 2

```java
public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        int[][] item = new int[N+1][2]; //weight, value
        for(int i=1; i<=N; i++){
            st = new StringTokenizer(br.readLine());
            item[i][0] = Integer.parseInt(st.nextToken()); // weight
            item[i][1] = Integer.parseInt(st.nextToken()); // value
        }

        int[] dp = new int[K+1];

        for(int i=1; i<=N; i++){
            for(int k = K; k>=item[i][0]; k--){
                dp[k] = Math.max(dp[k], dp[k-item[i][0]] + item[i][1]);
            }
        }

        System.out.println(dp[K]);
    }
}
```

# 회고

기본적인 문제지만 난 해결하지 못했다. 더 잘 익혀서 다음에는 풀 수 있도록 해야겠다.
