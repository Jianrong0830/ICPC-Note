---
tags: UVA
---

# UVA 11138 - Nuts and Bolts ([Link](https://onlinejudge.org/external/111/11138.pdf))
#二分圖匹配

## 想法
原程式碼是沒效率的二分圖匹配，因此要用好一點的演算法
[參考](https://web.ntnu.edu.tw/~algo/Matching.html#4)

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define MAXN 505
#define INF 0x3f3f3f3f3f3f
//#define int long long
using namespace std;

int t;
int n, m;
int map[MAXN][MAXN];
int mx[MAXN], my[MAXN], vist[MAXN];
vector<int> edge[MAXN];

//判斷當前x能否與y匹配
int dfs(int x){
    for(int y:edge[x]){
        if(!vist[y]){
            vist[y] = 1;
            if(my[y]==-1 || dfs(my[y])){
                mx[x] = y, my[y] = x;
                return true;
            }
        }
    }
    return false;
}

//計算最大匹配數
int bipartite_matching(){
    int ans = 0;
    for (int i = 1; i <= n; i++){
        memset(vist, 0, sizeof(vist));
        if(dfs(i))
            ans++;
    }
    return ans;
}

int main(void){
    cin >> t;
    for(int k=1; k<=t; k++){
        //初始化
        memset(mx, -1, sizeof(mx));
        memset(my, -1, sizeof(my));
        memset(map, 0, sizeof(map));
        for (int i = 0; i < MAXN; i++){
            edge[i].clear();
        }
        //輸入測資
        cin>>n>>m;
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= m; j++){
                cin >> map[i][j];
                if(map[i][j]==1)
                    edge[i].push_back(j);
            }
        }
        //執行算法並輸出
        cout << "Case " << k << ": a maximum of " << bipartite_matching() << " nuts and bolts can be fitted together\n";
    }
}
```