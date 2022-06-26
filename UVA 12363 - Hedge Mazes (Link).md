---
tags: UVA
---
# UVA 12363 - Hedge Mazes ([Link](https://onlinejudge.org/external/123/12363.pdf))
#Tarjan

## 想法
用Tarjan找到所有的橋，再將有連接的橋放在同一個並查集(大橋)
檢查兩點是否在同一個大橋，有的話輸出Y，否則輸出N

## Code
```c=
#include <iostream>
#include <string.h>
#include <vector>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int r, c, q;
vector<vector<int>> map;
int visited[200005], parent[200005];
int dfn[200005], low[200005];
int deep;
vector <pair< int, int >> bridge;
int set[200005];    //Disjoint Set

//Tarjan
void dfs(int id, int par){
    dfn[id] = low[id] = ++deep;
    parent[id] = par;
    for(auto& i : map[id]){
        if(!dfn[i]){
            dfs(i, id);
            low[id] = min(low[id], low[i]);
        }
        else if(i!=par){
            low[id] = min(low[id], dfn[i]);
        }
    }
}

void findBridge(){
    for (int i = 1; i <= r; i++){
        int p = parent[i];
        if(p>0 && low[i]>dfn[p]){   //i要有parent(才能形成橋)
            bridge.push_back(make_pair(p, i));
        }
    }
}

int findRoot(int i){
    if(set[i]==-1)
        return i;
    return set[i] = findRoot(set[i]);
}

//建立Disjoint Set
void build(){
    for (int i = 0;i<bridge.size(); i++){
        int u = bridge[i].first, v = bridge[i].second;
        set[v] = u;
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>r>>c>>q && r){
        //初始化
        map.clear();
        bridge.clear();
        clr(visited); clr(parent); clr(dfn); clr(low);
        memset(set, -1, sizeof(set));
        map.assign(r, vector<int>());
        deep = 0;
        //建立map
        while(c--){
            int a, b;
            cin >> a >> b;
            map[a].push_back(b);
            map[b].push_back(a);
        }
        //執行算法
        dfs(1, 0);
        findBridge();
        build();
        //輸出答案
        while(q--){
            int qa, qb;
            cin >> qa >> qb;
            if(findRoot(qa)==findRoot(qb)){
                cout << "Y\n";
            }
            else{
                cout << "N\n";
            }
        }
        cout << "-\n";
    }
}
```