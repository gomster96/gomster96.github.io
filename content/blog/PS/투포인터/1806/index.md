---
title: '백준 1806번 JAVA : 부분 합'
date: 2022-01-18 18:06:05
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1806

# 풀이전략

연속된 수들의 부분합 중 그 합이 S이상이 되는 것들 중, 가장 짦은 것의 길이를 구하는 문제이다.

1. 투 포인터 알고리즘을 사용하여 O(n)으로 문제를 해결 할 수 있다.
2. 합을 만드는 것이 불가능하다면 0을 출력해야한다.

# 코드

```java

import javax.rmi.ssl.SslRMIClientSocketFactory;
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
        int S = Integer.parseInt(st.nextToken());

        ArrayList<Integer> arr = new ArrayList<>();
        st = new StringTokenizer(br.readLine());
        long sum = 0;
        for(int i=0; i<N; i++){
            arr.add(Integer.parseInt(st.nextToken()));
            sum+=arr.get(i);
        }
        if(sum < S){
            System.out.println(0);
            System.exit(0);
        }
        int lt = 0, rt = 0;
        sum = arr.get(rt);
        int answer = Integer.MAX_VALUE;
        while(rt < N){
            if(sum < S){
                if(rt == N-1) break;
                sum += arr.get(++rt);
            } else {
                // sum이 S보다 크거나 같을 때
                answer = Math.min(answer, rt-lt+1);
                sum -= arr.get(lt++);
            }
        }
        System.out.println(answer);
    }
}


```

# 회고

- 투포인터 알고리즘을 사용해야 하는 것을 알면 쉽게 해결할 수 있는 문제다. 역시 어떤 알고리즘을 언제 사용할지 바로 파악하는 능력이 중요한 것 같다.
