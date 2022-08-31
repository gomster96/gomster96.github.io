---
title: '백준 1461번 JAVA : 도서관'
date: 2022-08-31 19:00:02
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1461

# 풀이전략

문제에서 주어진 조건을 정확하게 파악하는 것이 중요하다.

1. 세준이는 한 번에 최대 M권의 책을 들 수 있다.
   - 세준이는 책을 다 놓아두면 다시 0으로 돌아가서 책을 가지고 와야한다.
   - 한번 `M개의 책을 들고 이동할 때 가장 먼 곳의 거리 * 2` 가 마지막을 제외한 나머지 이동에서 필요한 걸음수이다.
2. 세준이는 책을 모두 제자리에 놔둔 후에는 다시 0으로 돌아올 필요가 없다.
   - 세준이가 맨 마지막에 이동할 곳은 0에서부터 가장 먼 곳이여야 한다.

이후 이 2가지 조건을 통해서 파악해야할 것이 있다.

- 음수와 양수 책들은 같이 한번에 이동해서는 안된다. 0을 지나기 때문에 경로가 낭비됨.
- 음수는 음수끼리, 양수는 양수끼리 한번에 이동하되 0에서부터 가까운 순서대로 서로 그룹핑 하는 것이 중요하다.

# 코드

```java

package backjoon.P1461;

import java.io.*;
import java.lang.reflect.Array;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    static int N, M;

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        Integer[] nums = new Integer[N];
        st = new StringTokenizer(br.readLine());
        ArrayList<Integer> minusNums = new ArrayList<>();
        ArrayList<Integer> plusNums = new ArrayList<>();
        int maxValue = 0;
        for (int i = 0; i < N; i++) {

            int num = Integer.parseInt(st.nextToken());
            maxValue = Math.max(maxValue, Math.abs(num));
            if (num < 0)
                minusNums.add(num);
            else if (num > 0)
                plusNums.add(num);
        }

        minusNums.sort((n1, n2) -> n1 - n2);
        plusNums.sort((n1, n2) -> n2 - n1);

        int cnt = 0;
        int answer = 0;


        if(!minusNums.isEmpty()){
            for (int i = 0; i < minusNums.size(); i++) {
                if (cnt == M) {
                    answer += 2 * Math.abs(minusNums.get(i - M));
                    cnt = 1;
                } else
                    cnt++;
            }
            answer += 2 * Math.abs(minusNums.get(minusNums.size() - cnt));
        }


        cnt = 0;
        if(!plusNums.isEmpty()){
            for (int i = 0; i < plusNums.size(); i++) {
                if (cnt == M) {
                    answer += 2 * Math.abs(plusNums.get(i - M));
                    cnt = 1;
                } else
                    cnt++;
            }
            answer += 2 * Math.abs(plusNums.get(plusNums.size() - cnt));
        }

        System.out.println(answer - maxValue);



        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

처음에 조건을 제대로 파악하지 못해 계속 문제를 제대로 해결하지 못했다. 그리디 알고리즘이였지만 결국 구현력을 판단하는 문제였다. 좋은 문제이므로 다시 복습하면 좋을 것 같다.
