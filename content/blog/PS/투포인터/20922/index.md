---
title: '백준 20922번 JAVA : 겹치는건 싫어'
date: 2022-08-29 18:06:06
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/20922

# 풀이전략

같은 원소가 K개 이하로 들어 있는 최장 연속 부분 수열의 길이를 구하는 문제이다.

- 투포인터 알고리즘을 통해, 새로운 숫자를 가르키는 포인터와, left의 기준이 되는 포인터 두개로 나눈다.
- 문제에서 메모리는 1024MB로 충분하므로, 100000개의 Array를 만들어서 새로운 숫자가 현재 수열에 들어올 수 있는지 확인하고, 들어올 수 없다면, left포인터부터 순차적으로 증가시켜 들어올 수 있게 만들어야하는 것이 문제 풀이의 핵심이다.

# 코드

```java

package backjoon.P20922;

import java.io.*;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        int lt = 0;
        int rt = -1;
        int[] numCnt = new int[100001];
        int answer = 1;
        List<Integer> nums = Arrays.stream(br.readLine().split(" "))
                                      .map(Integer::parseInt)
                                      .collect(Collectors.toList());

        for(int i=0; i<nums.size(); i++){
            int num = nums.get(i);
            while(numCnt[num] >= K && lt < rt){
                numCnt[nums.get(lt++)]--;
            }
            numCnt[num]++;
            rt++;
            answer = Math.max(rt - lt + 1, answer);
        }


        System.out.println(answer);

        bw.flush();
        bw.close();
        br.close();
    }
}


```

# 회고

- 투포인터 알고리즘이라는 것을 바로 생각했다면 쉽게 풀 수 있는 문제이다. 역시 알고리즘에 항상 준비되어서 언제든지 어떤 알고리즘인지 파악할 수 있도록 계속 준비해두는게 중요한 것 같다.
