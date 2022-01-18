---
title: '백준 1713번 JAVA : 후보 추천하기'
date: 2022-01-18 18:06:02
category: 'PS'
draft: true
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/1713

# 풀이전략

문제의 기능은 거의 LRU를 구현하는 것과 같다. 가장 많이, 최근에 추천받은 학생만 남긴 후 출력시 오름차순으로 출력해야한다.

1. 틀의 개수가 초과되면 추천횟수가 가장 많은 순으로, 게시된지 가장 최근인 순으로 정렬을 해야한다.
2. 마지막에 출력시 오름차순으로 출력해주어야 하는 것을 잊지말아야한다.

# 코드

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.*;

public class B_1713 {
    static class Pair{
        int num, cnt;

        public Pair(int num) {
            this.num = num;
            this.cnt = 1;
        }
    }
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());
        ArrayList<Pair> arr = new ArrayList<>();
        for(int i=0; i<M; i++){
            int nextNum = Integer.parseInt(st.nextToken());
            int numIdx = - 1;
            for(int j=0; j<arr.size(); j++){
                if(nextNum == arr.get(j).num){
                    numIdx = j;
                    break;
                }
            }
            if(numIdx == -1){
                if(arr.size() == N){
                    // 추천 개수가 가장 적은 학생을 삭제
                    int min = Integer.MAX_VALUE;
                    int minIdx = -1;
                    for(int j=0; j<arr.size(); j++){
                        if(min > arr.get(j).cnt){
                            min = arr.get(j).cnt;
                            minIdx = j;
                        }
                    }
                    arr.remove(minIdx);
                }
                arr.add(new Pair(nextNum));

            } else{
                arr.get(numIdx).cnt++;
            }
        }
        Collections.sort(arr, (e1, e2) ->{
            return e1.num - e2.num;
        });
        arr.forEach(el ->{
            System.out.print(el.num+" ");
        });
    }
}

```

# 회고

- pair를 만들어줘서 비교해주며 정렬하였다.
- 가장 나중의 게시글을 삭제하기위해 데이터는 뒤에서부터 넣고, 삭제할 게시글은 앞에서부터 찾았다.
- arr.remove()를 통해 해당 index의 데이터를 삭제할 수 있다.
