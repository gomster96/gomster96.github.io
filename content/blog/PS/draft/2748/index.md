---
title: '백준 2748번 JAVA : 피보나치 수2'
date: 2022-01-18 18:06:04
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2748

# 풀이전략

그냥 피보나치 수열을 구하는 문제이다. 워낙 비슷한 문제가 많으므로 쉽게 풀 수 있다. 단 메모이제이션을 이용하여 훨씬 빠른 시간복잡도로 문제를 해결할 수 있다.

# 코드

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class Main {
    static long[] dp;
    public static long Fibo(int n){
        if(dp[n] != -1) return dp[n];
        return dp[n] = Fibo(n-1) + Fibo(n-2);
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        dp = new long[N+1];
        for(int i=0; i<dp.length; i++){
            dp[i] = -1;
        }
        dp[0] = 0;
        dp[1] = 1;
        Fibo(N);
        System.out.println(dp[N]);
    }
}

```

# 회고
