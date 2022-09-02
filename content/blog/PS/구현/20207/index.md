---
title: '백준 20207번 JAVA : 달력'
date: 2022-09-02 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/20207

# 풀이전략

1년부터 365일까지 날짜가 쭉 이어져서 표현되어 있는 달력이다. 연속된 일정이 있을 때 연속된 일정을 표시해야한다.

1. 일정하나의 세로길이는 1, 하루의 폭은 1이다.
2. 하루에 여러 일정이 있다면, 세로의 길이는 그 갯수만큼 늘어난다. 즉 하루에 3개의 일정이 있으면 세로의 길이는 3이다.
3. 직사각형을 만들어야하기 때문에 가장 길이가 긴 세로를 기준으로 직사각형을 만든다.
4. 중간에 비어있는 칸이 발생하면, 새로운 직사각형을 만든다. (직사각형을 만들 기준은 길이가 366인 Array의 값을 추가해주는 식으로 만들면 된다.)

# 코드

```java

package backjoon.P20207;

import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder("");

        int N = Integer.parseInt(br.readLine());

        int S, E;
        int[] calender = new int[366];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            S = Integer.parseInt(st.nextToken());
            E = Integer.parseInt(st.nextToken());
            for(int j=S; j<=E; j++){
                calender[j]++;
            }
        }


        int answer = 0;
        int maxHeight = 0;
        int cnt = 0;
        for(int i=1; i<calender.length; i++){
            if(maxHeight < calender[i]){
                maxHeight = calender[i];
            } else if(calender[i] == 0){
                answer += maxHeight * cnt;
                cnt = 0;
                maxHeight = 0;
                continue;
            }
            cnt++;
        }
        answer += maxHeight * cnt;

        System.out.println(answer);

        bw.flush();
        bw.close();
        br.close();
    }
}

```

# 회고

간단한 구현문제.
