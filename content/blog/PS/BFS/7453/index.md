---
title: '백준 7453번 JAVA : 합이 0인 네 정수'
date: 2022-01-18 18:06:09
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/7453

# 풀이전략

4가지 배열이 주여질때, 각 배열의 합이 0이 되는 (a,b,c,d) 쌍의 개수를 구해야한다.

1. 배열의 크기가 4000개이지만 이를 완전탐색으로하면 4000\*4000\*4000\*4000 이라는 어마무시한 수가 나온다.
   - A,B 묶고 C,D묶어서 완전탐색을 하고, 이때 구한 합을 정렬하여 투포인터 알고리즘을 사용한다.
2. 어마무시한 수이므로 long을 써주어야한다.
3. ArrayList는 가급적이면 지양하자....
   - ArrayList는 배열의 크기가 꽉 차면 동적으로 배열을 다시 재생성하므로 기존 Array에 비해 느리다. 따라서 코딩테스트를 할 때는 가급적이면 Array를 사용해야겠다.

# 코드

```java


import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        long[] A = new long[N];
        long[] B = new long[N];
        long[] C = new long[N];
        long[] D = new long[N];
        StringTokenizer st = null;
        for(int i=0; i<N; i++){
            st = new StringTokenizer(br.readLine());
            A[i] = Integer.parseInt(st.nextToken());
            B[i] = Integer.parseInt(st.nextToken());
            C[i] = Integer.parseInt(st.nextToken());
            D[i] = Integer.parseInt(st.nextToken());
        }
        long[] abSum = new long[N*N];
        long[] cdSum = new long[N*N];
        int idx = 0;
        for(int i=0; i<N; i++){
            for(int j=0; j<N; j++){
                abSum[idx] = A[i] + B[j];
                cdSum[idx] = C[i] + D[j];
                idx++;
            }
        }
        Arrays.sort(abSum);
        Arrays.sort(cdSum);
        long answer = 0;
        int abP = 0;
        int cdP = idx-1;
        while (true) {

            long valAB = abSum[abP];
            long valCD = cdSum[cdP];
            long sum = valAB + valCD;
            if(sum > 0){
                cdP--;
            } else if(sum == 0){
                int abCnt = 0;
                int cdCnt = 0;
                while(abP < idx && abSum[abP] == valAB){
                    abP++;
                    abCnt++;
                }
                while(cdP >= 0 && cdSum[cdP] == valCD){
                    cdP--;
                    cdCnt++;
                }
                answer += (long) abCnt * cdCnt;
            } else{
                abP++;
            }
            if(abP >= idx || cdP < 0) break;;
        }

        System.out.println(answer);

    }
}



```

# 회고

아이디어는 빨리 나왔지만 관련해서 몰랐던 것들이 많아 문제를 푸는데 오래 걸렸다.

- ArrayList는 값이 적을 때만 사용한다. 이렇게 시간이 급할때는 가급적이면 사용하지 않는다.
- 4개가 너무 클때는 두개씩 쪼개서 문제를 해결하는것도 좋은 팁이다.
