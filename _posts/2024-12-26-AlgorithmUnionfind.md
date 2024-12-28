---
layout: single
title: "[알고리즘] Union-Find 유니온 파인드"
categories: Algorithm
tag: [Graph, Union-Find, Algorithm, Set, C++]
toc: true
---

# 유니온 파인드

유니온 파인드는 일반적으로 여러 노드가 있을 때 특정 2개의 노드를 연결해 1개의 집합으로 묶는 union 연산과 두 노드가 같은 집합에 속해 있는지를 확인하는 find 연산으로 구성된 알고리즘이다.

- **union 연산** : 각 노드가 속한 집합을 1개로 합치는 연산
    - 노드 a가 A집합에 속하고, 노드 b가 B집합에 속할 때, union(a, b)는 AUB를 의미한다.
- **find 연산** : 특정 노드 a에 관해 a가 속한 집합의 대표 노드를 반환하는 연산
    - 노드 a가 A집합에 속할 때, find(a)는 A집합의 대표 노드를 반환한다.
    - find 연산은 단순히 대표 노드를 찾는 역할만 하는 것이 아니라 그래프를 정돈하고 시간 복잡도를 줄여준다.
    - **find 연산의 작동 원리**
        1. 대상 노드 배열에 index 값과 value 값이 동일한지 확인한다.
        2. 동일하지 않으면 value 값이 가리키는 index 위치로 이동한다.
        3. 이동 위치의 index 값과 value 값이 같을 때까지 **1~2를 반복한다.** 반복이므로 이 부분은 재귀 함수로 구현한다.
        4. 대표 노드에 도달하면 재귀 함수를 빠져나오면서 거치는 모든 노드값을 대표 노드값으로 변경한다.

## 유니온 파인드의 원리

**(1) 유니온 파인드는 일반적으로 1차원 배열로 표현한다. 처음에는 노드가 연결되어 있지 않으므로 각 노드가 대표 노드가 된다. 각 노드가 모두 대표 노드이므로 배열은 자신의 인덱스 값으로 초기화한다.**

![Alt text](/assets/images/unionfind1.png)

**(2) 2개의 노드를 선택해 union 연산을 수행한다.**
- union(1, 4) : 1은 대표 노드, 4는 자식 노드로 union 연산을 하므로 배열[4]의 대표 노드를 1로 업데이트
- union(5, 6) : 5는 대표 노드, 6은 자식 노드로 union 연산을 하므로 배열[6]의 대표 노드를 5로 업데이트
이로 인해, 각각의 집합이였던, 1,4와 5,6은 하나로 합쳐졌다.

![Alt text](/assets/images/unionfind2.png)
    
- union(4, 6) : 4와 6은 대표 노드가 아니므로, 각 노드의 대표 노드를 찾아 올라간 다음 그 대표 노드를 연결한다. 아래 그림에서는 4의 대표노드 1과 6의 대표 노드 5를 연결하였다. 

![Alt text](/assets/images/unionfind3.png)

**(3) find 연산 진행**

![Alt text](/assets/images/unionfind4.png)

## 코드 구현

```cpp
#include <iostream>
using namespace std;

int parent[7];
void unionfunc(int a, int b);
int Find(int a);

void unionfunc(int a, int b){//유니온 연산 : 바로 연결 x 대표 노드끼리 연결
    a = Find(a);
    b = Find(b);

    if(a != b) parent[b] = a;
}

int Find(int a){ // 대표 노드를 찾아서 반환
    if(a == parent[a]) return a;
    else {
        return parent[a] = Find(parent[a]); // 경로 압축
    }
}

int main(void){
    for(int i=1; i<7; i++){
        parent[i] = i; //대표 노드를 자기 자신으로 초기화하기
    }

    unionfunc(1, 4); // parent[4] = 1
    unionfunc(5, 6); // parent[6] = 5
    unionfunc(4, 6); // parent[6] = 1
    Find(6);//index 6의 value를 대표 노드 값 1로 업데이트

    for(int i=1; i<7; i++){
        cout<<parent[i]<<" ";// 출력 결과 : 1 2 3 1 1 1
    }

    return 0;
}
```

## 유니온 파인드를 사용하는 문제들(feat. 백준)

[BOJ 1717 : 집합의 표현](https://www.acmicpc.net/problem/1717)
[BOJ 1976 : 여행 가자](https://www.acmicpc.net/problem/1976)
[BOJ 1043 : 거짓말](https://www.acmicpc.net/problem/1043)

## Reference
- Do it! 알고리즘 코딩 테스트 (c++) 
