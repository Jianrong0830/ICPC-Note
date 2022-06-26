---
tags: UVA
---
# UVA 12047 - Highest Paid Toll ([Link](https://onlinejudge.org/external/120/12047.pdf))
#Dijkstra

## 想法
找符合條件(總花費<p)中具有最大權重的邊
因此前、後兩段要是最短距離
正、逆向各做一次Dijkstra
遍歷一次所有的邊，邊i的總花費是dist1[u[i]]+weight[i]+dist2[v[i]]
若此總花費<p，則符合條件，將weight[i]加入priority_queue
最後的答案是priority_queue.top()

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
#define inf 0x3f3f3f
#define MAXN 10020
using namespace std;

struct Edge{
    int v, w; //vector, cost;
    Edge(): v(0), w(0) {}
    Edge(int _v, int _w): v(_v), w(_w) {}

    bool operator < (const Edge& other) const{
        return w > other.w;
    }
};

vector<Edge> edge1[MAXN], edge2[MAXN];
int dist1[MAXN], dist2[MAXN];
int vist1[MAXN], vist2[MAXN];

int T;
int n, m, s, t, p;

void init(){
    for (int i = 1; i <= n; i++){
        edge1[i].clear();
        edge2[i].clear();
    }
    memset(dist1, inf, sizeof(dist1));
    memset(dist2, inf, sizeof(dist2));
    memset(vist1, 0, sizeof(vist1));
    memset(vist2, 0, sizeof(vist2));
}

void dijkstra(int root, int dist[MAXN], int vist[MAXN], vector<Edge> edge[MAXN]){
    int cnt = 0;
    dist[root] = 0;
    int current;
    while(cnt<n){
        int min_dist = inf;
        for (int i = 1; i <= n; i++){
            if(!vist[i] && dist[i]<min_dist){
                min_dist = dist[i];
                current = i;
            }
        }
        for (auto& i:edge[current]){
            int node = i.v, weight = i.w;
            if(!vist[node] && dist[current] + weight < dist[node]){
                dist[node] = dist[current] + weight;
            }
        }
        vist[current] = 1;
        cnt++;
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> T;
    while(T--){
        init();     //初始化
        //輸入測資
        cin >> n >> m >> s >> t >> p;
        while(m--){
            int a, b, c;
            cin >> a >> b >> c;
            edge1[a].push_back({b,c});
            edge2[b].push_back({a,c});
        }
        //執行算法
        dijkstra(s, dist1, vist1, edge1);
        dijkstra(t, dist2, vist2, edge2);
        priority_queue<int> q;
        for (int i = 1; i <= n; i++){
            for(auto& j:edge1[i]){
                int start = i, end = j.v, weight = j.w;
                if(dist1[start]+dist2[end]+weight <= p){
                    q.push(weight);
                }
            }
        }
        //輸出答案
        if(q.size()==0)
            cout << -1 << '\n';
        else
            cout << q.top() << '\n';
    }
}
```
