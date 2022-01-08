---
title: '백준 2407번 JAVA : 조합'
date: 2022-01-08 21:22:04
category: 'PS'
draft: false
---

# 문제

<p align="center"><img src="1.png" height="600px" width="600px"></p>

백준 문제 링크 : https://www.acmicpc.net/problem/2407

# 풀이전략

1. 순열 조합의 식을 기억해야한다.

   `nCm = n-1Cm + n-1Cm-1`

2. 값이 매우 커질 수 있으니 long을 사용해야한다. -> 더 커질 경우 BigInteger.....

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.math.BigInteger;
import java.util.StringTokenizer;
//BigInteger에 대해 공부하기
public class B_2407 {
    public static  void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n, m;
        StringTokenizer st = new StringTokenizer((br.readLine()));
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());

        BigInteger[][] dp = new BigInteger[101][101];
        dp[1][0] = dp[1][1] = BigInteger.ONE;
        for(int i=0; i<101; i++){
            for(int j=0; j<=i; j++){
                if(i == j || j == 0){
                    dp[i][j] = BigInteger.ONE;
                }
                else{
                    dp[i][j] = dp[i-1][j].add(dp[i-1][j-1]);
                }
            }
        }
        System.out.println(dp[n][m]);
    }
}


```

# 회고

long으로 커버가 안되는 부분은 BigInteger라는 클래스르 사용해야 함을 알게 되었다.... 근데 정말 마음에 안든다. 왜 java는 long long이 없는것인가...?... 익혀야지... ㅎㅎ
