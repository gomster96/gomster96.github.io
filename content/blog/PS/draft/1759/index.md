---
title: '백준 1759번 JAVA : 암호 만들기'
date: 2022-01-18 18:06:01
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1759

# 풀이전략

암호를 만들되, 최소 한개의 모음 2개 이상의 자음이 있어야한다. 또한 암호는 증가하는 순서로 배열되어야한다.

1. 증가하는 순서로 배열되어야하기 때문에 암호들을 다 구한뒤 sort를 해주는 법도 있지만, 1차 array이므로 먼저 sort를 해준뒤 index를 변화하는 방향으로 문제를 해결한다.
2. 모음은 a,e,i,o,u 이다(y는 아니다...). 모음을 구별해야하므로 모음 배열을 따로 만들어준다.
3. 결국 모든 가짓수를 찾아야하므로 DFS로 해결하였다.

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class B_1759 {
    static StringBuilder sb = new StringBuilder();
    static ArrayList<Character> arr;
    static char[] vowel = {'a', 'e', 'i', 'o', 'u'};
    static int L, C;

    public static boolean isAnswer(String str){
        int vowelCnt = 0;
        int consonantCnt = 0;
        boolean addConsonanat = true;
        for(int i=0; i<str.length(); i++){
            for(int j=0; j<vowel.length; j++){
                addConsonanat = true;
                if(str.charAt(i) == vowel[j]){
                    addConsonanat = false;
                    vowelCnt++;
                    break;
                }
            }
            if(addConsonanat) consonantCnt++;
            if(vowelCnt >= 1 && consonantCnt >=2) return true;
        }
        return false;
    }

    public static void DFS(int idx){
        // 목적지 체크인
        sb.append(arr.get(idx));
        // 목적지인가?
        if(sb.length() == L){
            if(isAnswer(sb.toString())) System.out.println(sb.toString());
            sb.delete(sb.length()-1, sb.length());
            return;
        }
        // 갈수있는가?
        for(int i=idx+1; i<arr.size(); i++){
            // 간다
            DFS(i);
        }
        //체크아웃  맨 뒤에값 삭제하기
        sb.delete(sb.length()-1, sb.length());
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        L = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());
        arr = new ArrayList<>();
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<C; i++){
            arr.add(st.nextToken().charAt(0));
        }
        Collections.sort(arr);
        for(int i=0; i<arr.size(); i++){
            DFS(i);
        }

    }
}

```

# 회고

- 나는 DFS로 구한 후에 전체 string을 Answer인지 아닌지를 파악했지만, 다른분들의 풀이를 보니 DFS로 보낼때 **모음과 자음의 개수를 같이 보내주는 것이** 훨씬 효율적으로 보인다.
- 일차원 array이기 때문에 단순히 index를 늘리는 식으로 DFS를 보내주어 사용한다.
