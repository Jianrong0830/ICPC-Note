---
tags: Algorithm
---
# Topological Ordering

## 用途
針對DAG(有向無環圖)做拓樸排序

## 時間複雜度
O(V + E)

## 核心概念
DFS、Stack

## Code
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

vector<int> edge[MAXN];
int vist[MAXN];
int n, m;
stack<int> topo;

void dfs(int u){
    vist[u] = 1;
    for (int v:edge[u]){
        if(!vist[v]){
            dfs(v);
        }
    }
    topo.push(u);
}

int main(void){
    cin >> n >> m;
    int a, b;
    while(m--){
        cin >> a >> b;
        edge[a].push_back(b);
    }
    for (int i = 1; i <= n; i++){
        if(!vist[i])
            dfs(i);
    }
    while(!topo.empty()){
        cout << topo.top() << ' ';
        topo.pop();
    }
}
```


## 範例測資
input:
9 10
1 2
1 5
2 6
4 7
5 8
5 9
6 5
6 9
7 8
9 8

output:
4 7 3 1 2 6 5 9 8 