---
title: '크루스칼 알고리즘 (Kruskal Algorithm)'
date: 2022-02-10 15:33:19
category: 'Algorithm'
draft: false
---

# 인덱스 트리란

인덱스 트리는 포화 이진트리(모든 노드의 자식이 2인 이진트리)를 응용한 자료구조이다. 일반적으로 해당 두가지 연산을 M번 수행해야할 때 주로 사용한다.

1. 구간 left, right(left <= right>) 이 주어졌을 때 A[left] + A[left+1] + ... + A[right-1]+ A[right] 의 합을 구해라
2. i번째 수 A[i]를 V로 바꾸어라

이런 연산들이 여러번 수행할 때 인덱스 트리를 사용하면 모두 O(logN)에 수행할 수 있다.

## 인덱스 트리의 특징

- 포화 이진트리 형태의 자료구조이다.
- 리프노드 : 배열에 적혀있는 수 (N개)
- 내부노드 : 왼쪽 자식과 오른쪽 자식의 합 (N-1개)
- 리프노드의 개수가 N개 이상인 포화 이진트리는 높이가 최소 logN이다.
- 리프노드의 개수가 N개 보다 많아 비어있는 공간이 발생할 경우 구조에 지장이 가지 않도록 초기값을 설정한다. (주로 N의 두배만큼 설정하면 문제가 발생하지 않는다.)
  - leaf는 2^n 꼴로만 만들 수 있다.

### S값의 사용과 왼쪽 오른쪽 자식

- 리프 노드의 시작을 알리는 S값을 잘 사용하는 것이 중요하다.
  S값은 다음과 같이 구한다.

```java
int S = 1;
while(S< N){
    S *= 2;
}
```

이렇게 2의 제곱수 중 N보다 큰 딱 그수를 S값으로 지정한다.

현재 노드의 인덱스를 i라고 하면

- 인덱스트리의 왼쪽 자식의 인덱스는 tree[i*2]이다.
- 인덱스트리의 오른쪽 자식의 인덱스는 tree[i*2+1]이다.

# 인덱스 트리 구현

S값과 input들이 주어졌다고 가정하고 진행한다.

```java
class IndexTree{
    public long[] tree;
    int S;

    public void init(int S, long[] nInputs){
        this.S = S;

        // S값의 2배 +1 만큼 선언해야 루트인 1부터 시작해서 리프노드까지 배열을 만들 수 있다.
        tree = new long(S*2+1);

        for(int i=0; i<N; i++){
            tree[S+i] = nInputs[i]; // 리프노드의 값을 넣어준다.
        }
        for(int i=S-1; i>0; i--){
            tree[i] = tree[i*2] + tree[i*2+1]; // 내부노드의 값을 넣어준다.
        }
    }

    // 원하고싶은 구간(queryLeft ~ queryRight)의 합을 찾는 함수이다.
    public long query(long left, long right, int node, long queryLeft, long queryRight){
        // 현재노드의 구간합 left, right가 찾으려는 구간(queryRight, queryLeft)의 속하지 않으므로 패스
        if(left > queryRight || right < queryLeft) return 0;

        // 현재 노드의 구간합 left, right가 찾으려는 구간(queryRight, queryLeft)의 속하므로 return
        if(left >= queryLeft && right <= queryRight){
            return tree[node];
        }

        // 재귀적으로 자식들을 찾아가면서 value를 즉 구간합을 받아온다.
        long mid = (left + right) / 2;
        // 왼쪽 자식이므로 node*2를 통해 왼쪽 자식의 node index를 접근할 수 있다.
        long leftVal = query(left, mid, node*2, queryLeft, queryRight);
        // 오른쪽 자식이므로 node*2+1를 통해 왼쪽 자식의 node index를 접근할 수 있다.
        long rightval = query(mid+1, right, node*2+1, queryLeft, queryRight);

        return leftVal + rightVal;
    }

    // update의 경우 루트에서부터 리프로 내려오면서 값의 변화만큼 변경해준다.
    // 이때 변경해줄 diff를 구하기 위해 leaf의 값의 index는 S + [리프의 순서] -1 로 구할 수 있다.
    // 변경할 리프가 3번째 수라면 해당 리프는 tree[S+3-1]을 통해 접근할 수 있다.
    public void update(long left, long right, int node, long target, long diff){
        // left, right 범위에 target이 속하지 않을경우 그냥 return 해준다.
        if(left > target || right < target) return;

        // left, right 범위에 target이 속할 경우 diff만큼 노드의 값을 변경해준다.
        tree[node] += diff;
        if(left != right){
            long mid = (left + right) / 2;
            update(left, mid, node*2, target, diff);
            update(mid+1, right, node*2+1, target, diff);
        }
    }
}

```

# 정리

1. 인덱스트리에서 가장 중요한 부분은 역시 S값이다. S값은 리프노드의 시작을 알림과 동시의 S-1을 통해 내부노드의 끝을 알 수 있다.
2. query, update 를 할 때 재귀적으로 값을 구한다. 이중에서 가장 중요했던 부분은 찾을 값 또는 변경해줄 값이 left, right에 속하는지를 구분하는 것이 중요했다.
3. 인덱스트리의 node의 인덱스가 i라면 왼쪽자식의 인덱스는 i*2, 오른쪽자식의 인덱스는 i*2+1이다.
