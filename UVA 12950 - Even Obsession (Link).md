---
tags: UVA
---
# UVA 12950 - Even Obsession ([Link](https://onlinejudge.org/external/129/12950.pdf))
#Dijkstra

## 想法
用odd_dist存經過奇數邊的最短距離；even_dist存經過偶數邊的最短距離
更新的方法：
if(even_dist[current_v] + c < odd_dist[v]) odd_dist[v] = even_dist[current_v] + c;
if(odd_dist[current_v] + c < even_dist[v]) even_dist[v] = odd_dist[current_v] + c;

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define inf 0x3f3f3f
#define clr(k) memset(k,inf,sizeof(k))
#define MAXN 10020
using namespace std;

int n, m;
vector<pair<int,int>> map[MAXN];
int odd_dist[MAXN], even_dist[MAXN];

void init(){
    for (int i = 1; i <= n; i++)
        map[i].clear();
    clr(odd_dist); clr(even_dist);
}

void dijkstra(int root){
    deque<pair<int, int>> q;
    q.push_back({root, 0});
    even_dist[root] = 0;

    int cost;
    while(!q.empty()){
        pair<int, int> node = q.front(); q.pop_front();
        int current_v = node.first;
        for(auto i:map[current_v]){
            int v = i.first, c = i.second;
            //奇數邊
            cost = even_dist[current_v] + c;
            if(cost<odd_dist[v]){
                q.push_back({v, cost});
                odd_dist[v] = cost;
            }
            //偶數邊
            cost = odd_dist[current_v] + c;
            if(cost<even_dist[v]){
                q.push_back({v, cost});
                even_dist[v] = cost;
            }
        }
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>n>>m && n){
        init();     //初始化
        //輸入測資
        int a, b, c;
        while(m--){
            cin >> a >> b >> c;
            map[a].push_back({b, c});
            map[b].push_back({a, c});
        }
        //執行算法
        dijkstra(1);
        //輸出答案
        if(even_dist[n]>=inf)
            cout << -1 << '\n';
        else
            cout << even_dist[n] << '\n';
    }
}
```