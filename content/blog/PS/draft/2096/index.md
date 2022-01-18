---
title: '백준 2096번 JAVA : 내려가기'
date: 2022-01-18 18:06:06
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2096

# 풀이전략

내려갈 수 있는 최대값과, 최소 값을 구하는 문제이다. N은 100000개이므로 시간초과에 주의해야한다.

- maxSum, minSum을 즉 누적합을 만들며 만들며 for문을 진행하면 O(N)으로 만들 수 있다.
  - 각 위치에서 내려 갈 수 있는 최소수만을 더해오는 minSum
  - 각 위치에서 내려 갈 수 있는 최대수만을 더해오는 maxSum
- 시간초과를 벗어나기 위해 메모이제이션을 활용한다.

# 코드

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int[][] maxSumBoard =  new int[100001][3];
        int[][] minSumBoard =  new int[100001][3];
        for(int i=1; i<=N; i++){
            st = new StringTokenizer(br.readLine());
            int num = Integer.parseInt(st.nextToken());
            maxSumBoard[i][0] = Math.max(maxSumBoard[i-1][0],maxSumBoard[i-1][1]) + num;
            minSumBoard[i][0] = Math.min(minSumBoard[i-1][0],minSumBoard[i-1][1]) + num;
            num = Integer.parseInt(st.nextToken());
            maxSumBoard[i][1] = Math.max(maxSumBoard[i-1][0],Math.max(maxSumBoard[i-1][1], maxSumBoard[i-1][2])) + num;
            minSumBoard[i][1] = Math.min(minSumBoard[i-1][0],Math.min(minSumBoard[i-1][1], minSumBoard[i-1][2]) ) + num;
            num = Integer.parseInt(st.nextToken());
            maxSumBoard[i][2] = Math.max(maxSumBoard[i-1][1],maxSumBoard[i-1][2]) + num;
            minSumBoard[i][2] = Math.min(minSumBoard[i-1][1],minSumBoard[i-1][2]) + num;
        }
        int maxAnswer = Math.max(maxSumBoard[N][0],Math.max(maxSumBoard[N][1], maxSumBoard[N][2]));
        int minAnswer = Math.min(minSumBoard[N][0],Math.min(minSumBoard[N][1], minSumBoard[N][2]));
        System.out.println(maxAnswer + " " + minAnswer);
    }

}




```

# 회고

- 문제를 풀때 뭔가 더럽다 싶었지만,,,, for문을 추가하고싶지 않아 이렇게 풀었다. 더 좋은 풀이가 있을지 찾아봐야겠다.
- 처음에 N*N인줄알고 오래 걸렸다. 문제를 잘읽어야한다. 문제에서는 3*N이였다.
