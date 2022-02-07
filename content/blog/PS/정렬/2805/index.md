---
title: '백준 2805번 JAVA : 나무자르기'
date: 2022-01-18 18:06:04
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2805

# 풀이전략

선정할 수 있는 높이의 최대값 즉 가능한한 나무를 많이 살리는 방법을 찾는 것이다.

1. 정렬을 한뒤 height를 낮춰가며 구할 수 있는 나무의 길이의 합을 구한다.
   - 이렇게하면 정렬할때 사용되는 O(nlogn) 로 풀수 있다.

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
        int M = Integer.parseInt(st.nextToken());
        ArrayList<Integer> arr = new ArrayList<>();
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<N; i++){
            arr.add(Integer.parseInt(st.nextToken()));
        }
        arr.sort((a, b) -> b - a);

        long sum = 0;
        int treeIdx = 0;
        int height = arr.get(treeIdx++);
        int treeCnt = 1;
        while(sum < M){
            // height를 낮춘다.
            height--;

            // 기준선 보다 높은 나무의 수를 구한다.
            while(treeIdx < arr.size() && height < arr.get(treeIdx)){
                treeIdx++;
                treeCnt++;
            }
            // height를 낮추면, 기준선 보다 높은 나무의 수만큼 sum에 더해줘야한다.
            sum += treeCnt;
        }
        System.out.println(height);
    }
}


```

# 회고

- 문제를 풀다가 헷갈려서 주석을 적어주면서 풀었는데 훨씬 잘 풀 수 있었다. 이제 어려운 문제는 주석을 활용해야겠다.
