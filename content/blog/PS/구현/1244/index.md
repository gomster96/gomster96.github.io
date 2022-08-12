---
title: '백준 1244번 JAVA : 스위치 켜고 끄기'
date: 2022-08-12 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1244

# 풀이전략

스위치를 켜고 끄는 구현 문제이다.

1. 남, 녀 학생일 때마다 해야하는 기능이 다르다.
2. 스위치의 개수는 100개, 학생의 수는 100명이므로 N^2, N^3으로 진행해도 시간은 충분하다.
3. 결과 출력이 20개를 넘어갈 경우 출력을 20개 출력해 줄 때마다 줄 바꿈을 해주어야한다.

# 코드

```java


package backjoon.P1244;

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static int[] switches;
    static int N;

    public static int toggle(int num){
        return num == 1 ? 0 : 1;
    }

    public static void mStudent(int num){
        for(int i=num-1; i<N; i+=num){
            switches[i] = toggle(switches[i]);
        }
    }

    public static void girlStudent(int num, int jump){
        if(num-jump < 0 || num+jump >= N) return;

        if(switches[num-jump] == switches[num+jump]){
            switches[num-jump] = switches[num+jump] = toggle(switches[num-jump]);
            girlStudent(num, jump+1);
        }
    }

    public static void print(){
        for(int i=0; i<N; i++){
            System.out.print(switches[i]);
            if((i+1)%20 == 0){
                System.out.println("");
            } else System.out.print(" ");
        }
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;

        N = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine());
        switches = new int[N];
        for(int i=0; i<N; i++){
            switches[i] = Integer.parseInt(st.nextToken());
        }

        int T = Integer.parseInt(br.readLine());

        for(int i=0; i<T; i++){
            st = new StringTokenizer(br.readLine());
            int sex = Integer.parseInt(st.nextToken());
            int num = Integer.parseInt(st.nextToken());

            if(sex == 1) mStudent(num);
            else {
                switches[num-1] = toggle(switches[num-1]);
                girlStudent(num-1, 1);
            }
        }

        // 스위치 출력은 20개 씩

        print();


        bw.flush();
        bw.close();
        br.close();
    }
}


```

# 회고

간만에 자신감을 얻기 위해 실버 문제를 도전해보았는데, 역시 골드에서 열심히 털리다가 실버 문제를 푸니 가볍게 해결할 수 있었다. 골드 문제에서 해맨다고 너무 낙담하지 말고 계속 자신감 있게 문제를 해결해야겠다.
