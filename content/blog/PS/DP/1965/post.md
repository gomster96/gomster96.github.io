---
title: '백준 1965번 JAVA : 상자넣기'
date: 2022-08-16 21:22:07
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1965

# 풀이전략

1. 전형적인 가장 긴 증가하는 부분수열 (LIS) 문제이다. 이를 이해하고 문제를 해결하면 바로 해결할 수 있다. 
2. 문제에서 상자에 넣는다는 것은 결국 가장 긴 부분수열의 길이를 구하라는 것이다. 


# 코드

package backjoon.P1965;

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    static int N;
    static int[] dp;
    static int[] arr;
    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");


        N = Integer.parseInt(br.readLine());
        dp = new int[N];
        arr = new int[N];
        int LIS = 0;
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<N; i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }

        for(int i=0; i<N; i++){
            int idx = BinarySearch(arr[i], 0, LIS, LIS + 1);

            if(idx == -1){
                dp[LIS++] =arr[i];
            } else {
                dp[idx] = arr[i];
            }
        }

        System.out.println(LIS);
        bw.flush();
        bw.close();
        br.close();
    }

    private static int BinarySearch(int num,int start, int end, int size){
        int res = 0;
        while(start <= end){
            int mid = (start + end)/2;
            if(num <= dp[mid]){
                res = mid;
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
        if(start == size) return -1;
        return res;
    }
}
```

# 회고

이분탐색을 이용한 LIS 문제가 그렇게 크게 어렵지 않으므로, 제대로 익히도록 해야겠다.