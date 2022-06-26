---
tags: UVA
---
# UVA 11838 - Come and Go ([Link](https://vj.csgrandeur.cn/6f00a59aba81a88d06e84afe03b80ff0?v=1646470068))
#Tarjan

## 想法
只要找出每個城市是否有多於一個 SCC 即可

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int n, m;
vector<vector<int>> map;
int parent[2005];
int dfn[2005], low[1005];
int deep, cnt;

//Tarjan演算法
void Tarjan(int u, int par){
    dfn[u] = low[u] = ++deep;
    parent[u] = par;
    for(auto& v:map[u]){
        if(!dfn[v]){
            Tarjan(v, u);
            low[u] = min(low[u], low[v]);
        }
        else{
            low[u] = min(low[u], dfn[v]);
        }
    }
    if(dfn[u]==low[u])
        cnt++;
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>n>>m && n){
        //初始化
        map.clear();
        map.assign(n + 5, vector<int>());
        clr(parent); clr(dfn); clr(low);
        deep = cnt = 0;
        //輸入測資
        while(m--){
            int a, b, c;
            cin >> a >> b >> c;
            map[a].push_back(b);
            if(c==2){
                map[b].push_back(a);
            }
        }
        //搜尋SCC
        for (int i = 1; i <= n; i++){
            if(!dfn[i] && cnt<2){
                Tarjan(i, 0);
            }
        }
        //輸出答案
        cout << (cnt == 1) << '\n';
    }
}
```