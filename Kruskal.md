---
tags: Algorithm
---
# Kruskal

## 用途
找出無向圖的最小生成樹

## 時間複雜度
O(ElogE)

## 核心概念
並查集、Greedy

## Code
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

struct edge{
    int u,v,c;
};
bool cmp(edge a, edge b){
    return a.c<b.c;
}

vector<edge> Edge;
vector<edge> MST;
int parent[MAXN];
int n,m;

void init(){
    for(int i=0;i<MAXN;i++) parent[i]=-1;
}

int find_root(int id){
    if(parent[id]==-1) return id;
    return parent[id]=find_root(parent[id]);
}

void merge(int a,int b){
    int root_a=find_root(a),root_b=find_root(b);
    parent[root_b]=root_a;
}

void kruskal(){
    sort(Edge.begin(),Edge.end(),cmp);
    for(auto& i:Edge){
        int root_u=find_root(i.u),root_v=find_root(i.v);
        if(root_u!=root_v){
            MST.push_back(i);
            merge(i.u,i.v);
        }
    }
}

int main(void){
    init();
    cin>>n>>m;
    int a,b,c;
    while(m--){
        edge temp;
        cin>>a>>b>>c;
        Edge.push_back({a,b,c});
    }
    kruskal();
    for(auto& i:MST) cout<<i.u<<' '<<i.v<<' '<<i.c<<'\n';
}
```


## 範例測資
input:
7 12
1 2 23
2 3 20
3 4 15
2 7 1
3 7 4
1 7 36
4 7 9
1 6 28
6 5 17
5 4 3
6 7 25
5 7 16

output:
2 7 1
5 4 3
3 7 4
4 7 9
6 5 17
1 2 23