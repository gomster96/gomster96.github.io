---
title: '백준 1072번 JAVA : 게임'
date: 2022-01-18 18:06:08
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1072

# 풀이전략

게임의 승률이 향상 되기까지의 총 문제를 해결한 횟수를 구해야한다.

- 그냥 for문을 돌리면 X에 1,000,000,000 을 해버리면 시간초과가 나온다.
  - 이분탐색을 활용한다.
- 적어도 X값만큼 반복하면 반드시 승률이 바뀐다.
- 게임을 해도 승률이 변하지 않으면 -1을 출력한다.
  - 승률이 99%이상이면 아무리 게임을 하더라도 승률이 변하지 않는다.

# 코드

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static long X, Y, firstZ;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        X = Integer.parseInt(st.nextToken());
        Y = Integer.parseInt(st.nextToken());
        long t = 0;
        firstZ = (Y)*100/X;
        if(firstZ >= 99){
            System.out.println(-1);
        } else{
            binarySearch(1,X);
        }
    }
    static void binarySearch(long start, long end){
        long mid = 0, Z = 0;
        while(start <= end ){
            mid = (start + end) / 2;
            Z = (Y + mid) * 100 / (X+mid);

            if(Z > firstZ){
                end = mid - 1;
            } else {
                start = mid+1;
            }
        }
        System.out.println(start);
    }
}




```

# 회고

- 10억개를 백만개로 인식하여 시간초과가 나왔다. -> 잘보자
- 아직 이분탐색이 익숙하지 않으므로 정리해야겠다.
