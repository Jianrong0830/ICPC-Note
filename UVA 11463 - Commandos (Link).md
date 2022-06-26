---
tags: UVA
---
# UVA 11463 - Commandos ([Link](https://onlinejudge.org/external/114/11463.pdf))
#Floyd

## 想法
做一次floyd，然後求出起點 s 經過最遠的點後到終點 d 的最短路徑(最大的 dist[s][i] + dist[i][d])

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define inf 0x3f3f3f
#define clr(k) memset(k,inf,sizeof(k))
#define MAXN 105
using namespace std;

int t;
int n, m;
int s, d;
int dist[MAXN][MAXN];
void init(){
    for (int i = 0; i < n; i++){
        for (int j = 0; j <= n; j++){
            dist[i][j] = inf;
        }
        dist[i][i] = 0;
    }
}
void floyd(){
    for (int k = 0; k < n; k++){
        for (int i = 0; i < n; i++){
            for (int j = 0; j < n; j++){
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    for (int k = 1; k <= t; k++){
        cin >> n >> m;
        init();     //初始化
        //輸入測資
        int u, v;
        while(m--){
            cin >> u >> v;
            dist[u][v] = dist[v][u] = 1;
        }
        cin >> s >> d;
        //執行算法
        floyd();
        int farest = 0;
        for (int i = 0; i < n; i++){
            farest = max(farest, dist[s][i] + dist[i][d]);
        }
        //輸出答案
        cout << "Case " << k << ": " << farest << '\n';
    }
}
```