---
title: '서로소 집합 (Disjoint Set, Union-Find)'
date: 2022-01-24 15:33:08
category: 'Algorithm'
draft: false
---

<!-- <p align="center"><img src="1.png" height="600px" width="600px"></p> -->

# Union-Find

흔히 Union-Find로 알고있는 서로소 집합은 교집합이 공집합인 집합 들의 정보를 확인(Find)하고 조작(Union)할 수 있는 자료구조이다.

### Union 연산 Union(a, b)

- 서로 다른 두 원소 a, b가 있을 때 각 원소가 속한 집합을 하나로 합치는 연산이다.
- 따라서 원소 a는 집합 A에 속하고, 원소 b는 집합 B에 속할 때 Union(a,b)를 진행하면 두 집합의 AUB가 된다.

### Find 연산 Find(a)

- 원소 a에 대한 Find 연산은 a가 속한 집합의 대표번호를 반환하는 것이다.
- 속도 증가를 위해 경로 압축을 해줄 수 있다.

관련 코드는 아래 예제에서 보이도록 하겠다.

### Find 함수

- arr이라는 배열이 있을때 (arr배열에는 대표값이 들어있다.), 대표번호를 재귀적으로 찾아서 반환해주는 함수이다.
- 이떄 return부분에 `arr[n] = find(arr[n])` 을 해주었는데 이는 경로 압축이다.
  - find를 호출했을 경우 맨 처음의 n부터 대표 값 까지의 경로들의 arr[] 의 값들은 다 대표값이 된다.
  - 다시 같은 또는 이전에 호출했던 경로상의 값을 find했을 경우 바로 대표값을 return해줄 수 있게 된다.

```java
    static int[] arr;
    public static int find(int n){
        if(arr[n] == n) return n;
        return arr[n] = find(arr[n]);
    }

```

### Union 함수

Union함수는 결국 find를 통해서 대표값을 찾아주는 것을 이용하여, 대표값을 같게 만들어줌으로써 합집합을 구현하는 것이다.

```java
    public static void union(int a, int b){
        arr[find(a)] = find(b);
    }
```

- 이렇게 해줄시에 결국 a의 대표값의 b의 대표값으로 바뀌고, 이제 a가 대표값이었던 다른 요소들은 해당 요소들이 호출 될때마다 이전 대표값(union되기 이전의 대표값)에서 최신 대표값(union된 이후의 대표값)으로 바뀌게 된다.

# Reference
