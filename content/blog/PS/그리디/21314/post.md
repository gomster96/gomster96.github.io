---
title: '백준 21314번 JAVA : 민겸 수'
date: 2022-01-15 19:00:04
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/21314

# 풀이전략

민겸수라는 문제에 조건을 잘 파악해야한다. 문제에서 주어진 사진 M일떄 1, K일떄 5란 조건을 잘 파악해야한다.

1. 큰값을 구할 때와 작은 값을 구할 때 K의 역할이 다르다.
   - 큰 값을 구할 때 K는 5를 곱해줘야한다.
   - 작은 값을 구할 때 K는 그냥 뒤에 5를 추가해준다.
2. 핵심은 M이 몇개인지 세는 것이다. 이후 K를 어떻게 해줄지 결정한다.
3. M만 있을경우 (ex: M 이 5개)
   - 최대 값일 때는 11111
   - 최소 값일 때는 10000
4. 숫자가 크므로 String으로 처리해주어야한다.

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class B_21314 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();

        int i=0;
        StringBuilder maxVal = new StringBuilder("");
        StringBuilder minVal = new StringBuilder("");
        int mCnt = 0;
        while(str.length() > i){
            if(str.charAt(i) == 'M'){
                mCnt++;
            }
            else if(str.charAt(i)=='K'){
                if(mCnt != 0){
                    maxVal.append("5");
                    minVal.append("1");
                    for(int j=1; j<mCnt; j++){
                        maxVal.append("0");
                        minVal.append("0");
                    }
                    maxVal.append("0");
                    minVal.append("5");
                    mCnt = 0;
                }
                else{
                    maxVal.append("5");
                    minVal.append("5");
                }
            }
            i++;
        }
        if(mCnt != 0){
            maxVal.append("1");
            minVal.append("1");
            for(int j=1; j<mCnt; j++){
                maxVal.append("1");
                minVal.append("0");
            }
        }
        System.out.println(maxVal);
        System.out.println(minVal);
    }
}


```

# 회고

M이 연속일 때 11111로 해주어야한다는것.... 흠 개인적으로 약간 지엽적이긴 하지만 그럼에도 불구하고 풀었어야하는 부분이였다. 이런 부분까지 생각할 수 있도록 잘 복습해야겠다.
