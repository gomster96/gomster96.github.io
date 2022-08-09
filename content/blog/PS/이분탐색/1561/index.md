---
title: '백준 1561번 JAVA : 놀이공원'
date: 2022-08-09 18:06:08
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1561

# 풀이전략

1. 모든 아이가 탈 수 있는 전체 운행시간을 이분탐색의 탐색할 대상으로 찾는것이 중요한 알고리즘이다.
2. (전체시간 / 놀이기구의 운행시간) 이 계산식을 통해, 전체 운행시간에 따라 각 놀이기구 들이 몇명씩 태울 수 있는지 확인할 수 있다. 이후에 태운 모든 아이들의 합이 N값과 같을 때, 이때가 우리가 원하는 답이다.
3. 전체시간을 구한 후, N명의 아이들이 타야 한다. 이때 마지막 아이를 구하는 방법은 ((전체시간 % 놀이기구 운행시간) == 0) 이 조건을 만족하는 놀이기구들이 가장 마지막에 아이들이 타는 놀이기구이다.

# 코드

```java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static long N;
    static int M;
    static int[] time;
    static int max;

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] sarr = br.readLine().split(" ");
        N = Long.parseLong(sarr[0]);
        M = Integer.parseInt(sarr[1]);

        for (int i = 1; i <= M; i++) {
            time[i] = Integer.parseInt(sarr[i - 1]);
            max = Math.max(max, time[i]);
        }

        if (N <= M) {
            bw.write(N + "\n");
            bw.flush();
            return;
        } else {
            long t = binSearch(0, (N / M) * max);

            long cnt = M;
            for (int i = 1; i <= M; i++) {
                cnt += (t - 1) / time[i];
            }

            for (int i = 1; i <= M; i++) {
                if (t % time[i] == 0)
                    cnt++;

                if (cnt == N) {
                    bw.write(i + "\n");
                    bw.flush();
                    return;
                }
            }
        }

        bw.flush();
        bw.close();
        br.close();
    }

    public static long binSearch(long left, long right) {

        long pl = left;
        long pr = right;

        do {
            long pc = (pl + pr) / 2;

            long sum = M;
            for (int i = 1; i <= M; i++) {
                sum += pc / time[i];
            }

            if (sum < N)
                pl = pc + 1;
            else
                pr = pc - 1;

        } while (pl <= pr);

        return pl;
    }
}


```

# 회고

- 이분탐색임을 생각하지 못했다. 매우 중요한 이분탐색 문제이므로 잘 복습해야겠다.

# Reference

- https://lotuslee.tistory.com/91
