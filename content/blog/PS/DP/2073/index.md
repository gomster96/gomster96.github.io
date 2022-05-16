---
title: '백준 2073번 JAVA : 수도배관 공사'
date: 2022-05-16 16:40:02
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2073

# 풀이전략

1. 전형적인 배낭문제이다. 수도관들은 각각 최대 1번씩만 사용할 수 있다.
2. for 문을 사용하여 전체 수도관들에 대하여 다 한번씩 돌려본다.
3. 가장 작은 capacity가 전체 capacity가 된다는 것이 이 문제의 다른 점이다.

# 코드

```java

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        int distance, town;
        st = new StringTokenizer(br.readLine());

        distance = Integer.parseInt(st.nextToken());
        town = Integer.parseInt(st.nextToken());
        int[] dp = new int[distance+1];
        dp[0] = Integer.MAX_VALUE;

        for(int i=0; i<town; i++){
            int length, capacity;

            StringTokenizer pipe = new StringTokenizer(br.readLine());

            length = Integer.parseInt(pipe.nextToken());
            capacity = Integer.parseInt(pipe.nextToken());

            // 해당 pipe 의 길이와 같은동안 까지 반복하는 아이디어가 중요하다.
            for(int y=distance; y>=length; y--){
                // 모든 수도관들을 각각 한번식 넣어본다.
                // 그중 가장 큰 capacity를 채택한다.
                dp[y] = Math.max(dp[y], Math.min(capacity, dp[y-length]));

            }
        }
        System.out.println(dp[distance]);
    }
}

```

# 회고

모든 수도관들을 다 한번씩 적용해보아, 그 값들 중 최대 값을 구하는 아이디어가 매우 좋은 아이디어인것 같다.

# Reference

- https://kwoncorin.tistory.com/83
