---
title: '백준 2143번 JAVA : 두 배열의 합'
date: 2022-01-18 18:06:07
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2143

# 풀이전략

두 배열을 합하여 구할 수 있는 쌍의 개수를 구하는 문제이다.

- 각 배열들의 합으로 다시 부분집합을 만들어 이를 투포인터 알고리즘을 사용하여 풀면 된다.
  - 각 배열의 합은 순차적으로 더하는 것만 고려한다. 즉 연속된 부분배열을 사용해야한다.
- 1000 \* 1000000을 하면 값이 int값을 넘어가므로 long으로 만들어야한다.

# 코드

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class Main {

//    public static void getSubset( int[] arr, int idx, int sum, int cnt,ArrayList<Integer> subset){
//        if(idx == arr.length){
//            if(cnt!=0) subset.add(sum);
//            return;
//        }
//        getSubset(arr, idx+1, sum+arr[idx], cnt+1, subset);
//        getSubset(arr, idx+1, sum, cnt, subset);
//    }



    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        int N = Integer.parseInt(br.readLine());
        int[] a = new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine());
        ArrayList<Integer> subA = new ArrayList<>();
        for(int i=0; i<N; i++){
            a[i] = Integer.parseInt(st.nextToken());
        }
        for(int i=0; i<N; i++){
            int sum = a[i];
            subA.add(sum);
            for(int j=i+1; j<N; j++){
                sum+=a[j];
                subA.add(sum);
            }
        }

        N = Integer.parseInt(br.readLine());
        int[] b = new int[N];
        st = new StringTokenizer(br.readLine());
        ArrayList<Integer> subB = new ArrayList<>();
        for(int i=0; i<N; i++){
            b[i] = Integer.parseInt(st.nextToken());
        }
        for(int i=0; i<N; i++){
            int sum = b[i];
            subB.add(sum);
            for(int j=i+1; j<N; j++){
                sum+=b[j];
                subB.add(sum);
            }
        }
        Collections.sort(subA);
        Collections.sort(subB);
        int pA = 0, pB = subB.size()-1;
        long answer = 0;
        while(true){
            int aVal = subA.get(pA);
            int bVal = subB.get(pB);
            long sum = aVal + bVal;
            if(sum > T){
                pB--;
            } else if(sum == T){
                int aPCnt = 0;
                int bPCnt = 0;
                while(pA < subA.size() && subA.get(pA) == aVal){
                    aPCnt++;
                    pA++;
                }
                while(pB >=0 && subB.get(pB) == bVal){
                    bPCnt++;
                    pB--;
                }

                answer += (long) aPCnt * bPCnt;
            } else {
                pA++;
            }
            if(pB < 0 || pA >= subA.size() ) break;
        }
        System.out.println(answer);
    }

}




```

# 회고

이 문제를 풀면서 너무 많은 실수를 하여 풀이가 꽤 오래 걸렸다. 실수를 반복하지 않도록 여기서 반성한다.

- subSet을 제대로 인식하지 않음 (각 부분집합이 연속된 부분배열이 아닌 그냥 부분집합이라고 생각함, 주석친 부분)
- if(pB < 0 || pA >= subA.size() ) break; 여기부분에서 (pA >= 를 > 로 적어 틀림) -> 조건문제
- sum과 answer이 long이 될 수 있음을 인식하지 않음
