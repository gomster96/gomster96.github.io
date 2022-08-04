---
title: '백준 16472번 JAVA : 고냥이'
date: 2022-08-04 23:58:07
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/16472

# 풀이전략

1. 전형적인 투 포인터 문제이다. 시작점과 끝점을 통해, 최대길이를 구하고, 구하는 도중 현재 몇개의 알파벳을 사용하였는지 계속 확인해가며 문제를 해결한다.

2. 알파벳은 총 26개이기 때문에 그렇게 배열을 선언하여 체크해나가면 조금 더 쉽게 문제를 접근 할 수 있다.

3. 새로운 알파벳을 추가할 때, 기존의 알파뱃을 삭제해야한다면, 연속된 기존 알파벳을 삭제해야한다.

# 틀린 코드

처음엔 문제를 다음과 같이 접근하였다. 새로운 문자가 나오면, 기존문자의 사이즈를 확인하면서 문자를 바꾸어 주었다. 하지만 내 풀이에서는 반례가 존재한다.

N은 3이고, line은 abcadbbbbb일 때이다.

내 로직에서는 alphas ArrayList에 순차적으로 다음과 같이 저장한다.

a b c

d b c

이렇게 될 시에 위에 예시를 기준으로 나의 풀이의 답은 6이 된다. 3번째 idx의 a는 포함되지 않기 때문이다.

```java

public class Main {

    public static class Pair{
        int idx, character;

        public Pair(int idx, int character) {
            this.idx = idx;
            this.character = character;
        }
    }

    public static boolean isAlphaExist(ArrayList<Pair> alphas, char nextAlpha){
        return alphas.stream().anyMatch(el -> el.character==nextAlpha);
    }

    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        String line = br.readLine();
        ArrayList<Pair> alphas = new ArrayList<>();
        int firstAlphaIdx = 0;
        int alpahSize = 0;
        int maxLength = 1;
        for(int i=0; i<line.length(); i++){

            if(isAlphaExist(alphas, line.charAt(i))) continue;

            if(alpahSize < N){
                alpahSize++;
                alphas.add(new Pair(i, line.charAt(i)));
                continue;
            }

            int curLength = i - alphas.get(firstAlphaIdx).idx;
            if(curLength > maxLength) maxLength = curLength;

            alphas.set(firstAlphaIdx, new Pair(i, line.charAt(i)));
            firstAlphaIdx = (firstAlphaIdx + 1) % N;
        }

        int curLength = line.length() - alphas.get(firstAlphaIdx).idx;
        if(curLength > maxLength) maxLength = curLength;

        System.out.println(maxLength);

        bw.flush();
        bw.close();
        br.close();
    }
}


```

# 옳은 풀이

따라서 풀이를 다음과 같이 바꾸었다.

```java
package backjoon.P16472;

import java.io.*;
import java.util.ArrayList;
import java.util.StringTokenizer;


public class Main {




    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        String line = br.readLine();

        int alpahSize = 0;
        int maxLength = 1;
        int[] ch = new int[26];
        int lo = 0;
        for(int i=0; i<line.length(); i++){


            int nextAlpha = line.charAt(i) - 'a';

            if(ch[nextAlpha] == 0){
                alpahSize++;
            }
            ch[nextAlpha]++;
            while(alpahSize > N){
                int eraseNum = line.charAt(lo) - 'a';
                ch[eraseNum]--;
                lo++;
                if(ch[eraseNum]==0){
                    alpahSize--;
                }
            }

            maxLength = Math.max(maxLength, i-lo+1);
        }



        System.out.println(maxLength);

        bw.flush();
        bw.close();
        br.close();
    }
}
```

# 회고

내가 아직 투포인터에 미숙하여 이런 문제가 발생한 것 같다. 조금 더 깊은 이해로, 문제를 접근하여 잘 해결하도록 해야겠다.

투포인터로써 당연히 하지 말아야할 실수를 했기 때문에 반성해야겠다.
