---
title: '백준 2011번 JAVA : 암호 코드'
date: 2022-05-16 16:40:02
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/2011

# 풀이전략

가짓수를 찾는 문제이다. 나는 먼저 규칙을 찾으려고 노력했다. 또한 반례가 상당히 많으므로 이에 대해 깊이 생각하고 천천히 찾아야한다.

- 문자가 1개일 때 경우 : 1개
- 문자가 2개일 때 경우 : 2개
- 문자가 3개일 때 경우 : 121 일 경우 => 1 + 21 & 12 + 1 & 1 + 2 + 1
  이 된다. 이에 대한 구성을 잘 보면 결국 아래와 같은 점화식을 도출 할 수 있다.

> A(n+2) = A(n+1) + A(n)

하지만 몇가지 반례가 있다.

1. 6이상으로 끝나는 숫자가 있을 경우, (36, 46, 56 등)
2. 0의 존재
   - 1010
   - 00
   - 30, 40, 50, 60
   - 0123
   - 다양한 문제가 있다.
3. 3이상의 숫자 일 경우 (13, 23 은 가능, 허나 31, 33 은 불가)

# 코드

```java

package backjoon.P2011;

import java.io.*;
import java.util.Objects;
import java.util.StringTokenizer;

public class Main {

    static int[] dp = new int[5001];
    static int divider = 1000000;
    public static int getDp(int n){
        if(n < 0) return 0;
        if(n == 0) return 1;
        if(n == 1) return 1;
        if(n == 2) return 2;
        if(dp[n] != 0) return dp[n];

        return dp[n] = (getDp(n-1) + getDp(n-2)) % divider;
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String line = br.readLine();

        long answer = 1;
        int preNum = 0;
        int n = 0;
        if(Objects.isNull(line)){
            System.out.println(0);
            return;
        }
        for(int i=0; i<line.length(); i++){
            int num = line.charAt(i) - '0';

            if(num == 0){
                if(preNum == 0 || preNum >= 3){
                    System.out.println(0);
                    return;
                }
                if(preNum < 3){
                    answer = (answer * getDp(n-1)) % divider;
                    n = preNum = 0;
                    continue;
                }
            }


            if(num > 6){
                if(preNum == 1){
                  n++;
                  answer = (answer * getDp(n)) % divider;
                } else if(preNum > 1){
                    answer = (answer * getDp(n)) % divider;
                }
                n = 0;
            } else if(num > 2){
                n++;
                answer = (answer * getDp(n)) % divider;
                n = 0;
            } else if(num == 1 || num == 2){
                n++;
            }

            preNum = num;
        }

        if(n != 0){
            answer = (answer * getDp(n)) % divider;
        }

        System.out.println(answer);

        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

반례가 너무 많아서 어려운 문제였다...... 반례를 잘 찾을 수 있도록 집중을 해야겠다.
