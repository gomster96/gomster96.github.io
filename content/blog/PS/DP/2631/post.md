---
title: '백준 2631번 JAVA : 줄세우기'
date: 2022-08-17 21:22:07
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2631

# 풀이전략

1. 전형적인 가장 긴 증가하는 부분수열 (LIS) 문제이지만 한가지 함정이 있다. 즉 결국 움직여야 할 수는 `전체 학생수 - LIS의 길이` 가 된다. 
2. 결국 아이들의 순서를 맞추는 것이므로, LIS를 구하고, 이후 LIS를 제외한 나머지 부분들을 배치시켜주면 결국 이것이 가장 최적화된 횟수이기 때문이다.

# 코드

```java

package backjoon.P2631;

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    static int N;
    static int[] arr, dp;



    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        N = Integer.parseInt(br.readLine());
        arr = new int[N];
        dp = new int[N];

        for(int i=0; i<N; i++){
            arr[i] = Integer.parseInt(br.readLine());
        }

        int LIS = 0;

        for(int i=0; i<N; i++){
            int idx = BinarySearch(arr[i], 0, LIS, LIS+1);
            if(idx == -1){
                dp[LIS++] = arr[i];
            } else {
                dp[idx] =arr[i];
            }
        }

        System.out.println(N - LIS);

        bw.flush();
        bw.close();
        br.close();
    }

    private static int BinarySearch(int num, int start, int end, int size){
        int res = 0;
        while(start <= end){
            int mid = (start + end) / 2;
            if(num <= dp[mid]){
                res = mid;
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
        if(start == size) {
            return -1;
        }
        return res;
    }
}
```

# 회고

이분탐색을 이용한 LIS 문제가 그렇게 크게 어렵지 않으므로, 제대로 익히도록 해야겠다.