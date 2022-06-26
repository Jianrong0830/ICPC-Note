---
tags: Algorithm
---
# Dijsktra

## 用途
針對有、無權重的有向圖做出單點全源最短路徑

## 時間複雜度
O(V + ElogV) 

## 核心概念
Priority Queue

## Code
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

struct edge{
    int v,c;
};

struct node{
    int id,cost;
    bool operator < (const node& other) const{
        return cost<other.cost;
    }
};

vector<edge> Edge[MAXN];
int dist[MAXN];
int n,m;

void dijsktra(){
    priority_queue<node> q;
    q.push({1,0});
    dist[1]=0;
    while(!q.empty()){
        int u=q.top().id;
        q.pop();
        for(auto& temp:Edge[u]){
            int v=temp.v,c=temp.c;
            if(dist[u]+c<dist[v]){
                dist[v]=dist[u]+c;
                q.push({v,dist[v]});
            }
        }
    }
}

int main(void){
    memset(dist,INF,sizeof(dist));
    cin>>n>>m;
    int a,b,c;
    while(m--){
        cin>>a>>b>>c;
        Edge[a].push_back({b,c});
    }
    dijsktra();
    cout<<dist[n];
}
```


## 範例測資
input:
6 9
1 2 1
1 3 12
2 3 9
2 4 3
3 5 5
4 3 4
4 5 13
4 6 15
5 6 4

output:
17