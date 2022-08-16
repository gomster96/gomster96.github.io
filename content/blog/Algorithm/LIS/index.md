---
title: '가장 긴 증가하는 부분 수열(Longest Increasing Subsequence, LIS)'
date: 2022-08-16 15:33:08
category: 'Algorithm'
draft: false
---

# LIS

- LIS는 Longest Increasing Subsequence의 약자로, 가장 긴 증가하는 부분 수열이라는 뜻이다.

- Dynamic Programming 알고리즘이다.

- for문을 두번 사용하여 O(n^2)으로 해결하는 방법이 있고, 이분탐색을 사용하여 최장 길이를 확인하는 배열을 만들고, O(nlogN)으로 해결하는 방법이 있다. 두 방법 모두 익히도록 하자.

- 해당 [블로그의 글](https://sskl660.tistory.com/89)을 참고하였다. 훨씬 더 상세하게 좋은 설명이 되어있으므로 보고 익히도록 하자.

# O(n^2) 풀이

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class LIS {

    static int N;
    static int[] arr;

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        arr = new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        // 각 위치에서 LIS 이 값을 기준으로 이전까지의 가장 긴 부분 수열의 길이를 저장한 DP테이블을 정의
        int[] dp = new int[N];

        // 최대 LIS의 값 최소값은 1이므로, 1로 초기화
        int max = 1;

        for (int i = 0; i < N; i++) {
            // 본인 그 자체가 LIS로써 가질 최솟값이므로 본인의 길이 1 이 항상 초기 값이 되어야 함.
            dp[i] = 1;
            // 현재 원소 i 이전의 dp테이블을 확인하며 LIS를 구함
            for (int j = 0; j < i; j++) {
                if (arr[j] < arr[i]) {
                    // dp 테이블에 저장된 LIS를 바탕으로, 현재 dp[i]의 max값 최신화
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            max = Math.max(max, dp[i]);
        }
        System.out.println(max);
    }
}

```

# 이분탐색을 활용한 O(NlogN)풀이

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class LIS_Binary {
    static int N;
    static int[] arr, dp;


    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        arr = new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        // 각 위치에서 LIS 이 값을 기준으로 이전까지의 가장 긴 부분 수열의 길이를 저장한 DP테이블을 정의
        int[] dp = new int[N];

        // 처음에 저장된 원소는 없으므로, LIS = 0 이다
        int LIS = 0;

        for (int i = 0; i < N; i++) {
            int idx = BinarySearch(arr[i], 0, LIS, LIS+1);

            // 찾지 못할 경우 == 새로 추가해야할 경우
            if(idx == -1){
                // 가장 마지막 위치에 원소를 삽입하고, LIS의 길이를 늘린다.
                dp[LIS++] = arr[i];
            } else {
                // 값을 찾은 경우 교체해준다.
                dp[idx] = arr[i];
            }
        }
    }

    private static int BinarySearch(int num, int start, int end, int size) {
        int res = 0;
        while (start <= end) {
            // 중앙 값을 찾음 -> 이분탐색의 기본
            int mid = (start + end) / 2;

            // 현재 선택된 원소가, mid 값 보다 작거나 같으면, 더 앞부분을 탐색한다.
            if (num <= dp[mid]) {
                res = mid;
                end = mid - 1;
            } else {
                // 현재 선택된 원소가 mid 값 원소보다 크면, 뒷 부분을 탐색한다.
                start = mid + 1;
            }
        }
        // dp 테이블에서 삽입될 위치를 못찾을 경우 ( 모든 수들 보다 클 경우)
        if (start == size) {
            return -1;
        } else {
            return res;
        }
    }
}


```

# Reference

- https://sskl660.tistory.com/89
