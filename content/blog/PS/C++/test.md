---
title: 'C++TEST'
date: 2020-6-14 16:21:13
category: 'PS'
draft: false
---

## 문제

![](https://images.velog.io/images/gomster_96/post/58bffe46-5423-4b49-8af8-44c0448394a0/image.png)
문제링크 : https://www.acmicpc.net/problem/13305

## 풀이전략

1. 제일 왼쪽도시에서 제일 오른쪽 도시로 이동하는동안 최소의 비용을 계산해야한다. 즉 기름을 사는데 사용할 돈의 최소값이다.
2. 다음 도시가 현재 도시보다 용량이 작은 기름이 나올때까지 계속 다음 도시를 확인한다. 만약 더 작은 기름이 나온다면 이전까지 지나왔던 도로의 길 \* 기름값 만큼 더해주며 이 과정을 반복하며 목적지까지 간다.
3. long long을 써주는 것을 주의해야한다.

## 코드

```cpp
#include<cstdio>
#include<vector>
using namespace std;



int main(){

    // freopen("../input.txt","rt",stdin);
    int N;
    scanf("%d",&N);
    vector<int> road;
    vector<int> oil;
    int tmp;
    for(int i=0; i<N-1; i++){
        scanf("%d",&tmp);
        road.push_back(tmp);
    }
    for(int i=0; i<N; i++){
        scanf("%d",&tmp);
        oil.push_back(tmp);
    }


    long long res = 0;
    // 시작점처리해준다.
    int oilPrice = oil[0];
    long long roadCnt = road[0];
    // 가장 왼쪽부터 오른쪽 까지 다음 오일값이 작은 값이 나올때까지 길을 더해준다.
    // 더 작은 값이 나온다면 이제 지금까지 저장된 값을 res에 추가해주고, 새로운 oilPrice로 for문을 진행한다.
    for(int i=1; i<N; i++){
        if(oilPrice < oil[i]){
            roadCnt += road[i];
        }
        else{
            res += oilPrice * roadCnt;
            roadCnt = road[i];
            oilPrice = oil[i];
        }
    }
    res += oilPrice * roadCnt;

    printf("%lld\n",res);
    return 0;
}



```

## 소감

항상 long long을 주의해야한다! 또한 문제가 길었지만 그럼에도 핵심만 잡으면 쉽게 해결할 수 있다.
