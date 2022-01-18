---
title: '백준 2003번 JAVA : 수들의 합'
date: 2022-01-18 18:06:03
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2003

# 풀이전략

수열의 부분합이 M이 되는 경우의 수를 구해야한다.

1. 투 포인터 알고리즘을 사용하여, 왼쪽, 오른쪽 포인터를 만들어 값이 작으면 오른쪽을++, 값이 크면 왼쪽을 ++ 하여 수열의 합을 구한다.

# 코드

```java

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        ArrayList<Integer> arr = new ArrayList<>();
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<N; i++){
            arr.add(Integer.parseInt(st.nextToken()));
        }
        int lt = 0, rt = 0;
        int sum = arr.get(lt);
        int answer = 0;
        while(!(rt == N-1 && sum < M)){
           if(sum < M) sum += arr.get(++rt);
           else {
               if(sum == M) answer++;
               sum -= arr.get(lt++);
           }
        }
        System.out.println(answer);
    }
}


```

# 회고

- 전위연산자, 후위연산자를 통해 코드를 더 짧게 만들었다.
- 투포인터 알고리즘은 중요하므로, 언제 써야할지 잘 파악해야한다.
