---
tags: UVA
---
# UVA 10805	- Cockroach Escape Networks ([Link](https://onlinejudge.org/external/108/10805.pdf))
#Floyd

## 想法
用Floyd，但是因為起點可能在trail上，因此trail也要算是節點
另設一個變數trail，範圍是n~n+trail數量-1，意思是trail的編號，在記錄道路時不斷遞增1，最終的值為 nest數量+trail數量+1
針對每個nest和trail跑floyd
最後找答案時起點範圍為 0~trail-1，終點範圍為 0~n-1 (因為最後一定要跑進nest裡面)

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#include <algorithm>
#define int long long
#define inf 0x3f3f3f
#define clr(k) memset(k,inf,sizeof(k))
#define MAXN 510
using namespace std;

int t;
int n, m;
int dist[MAXN][MAXN];

void init(){
    for (int i = 0; i < MAXN; i++){
        for (int j = 0; j < MAXN; j++){
            dist[i][j] = inf;
        }
        dist[i][i] = 0;
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    for (int c = 1; c <= t; c++){
        init();     //初始化
        //輸入測資
        cin >> n >> m;
        int a, b;
        int trail = n;
        for (int i = 0; i < m; i++){
            cin >> a >> b;
            //將trail新增至節點
            dist[a][trail] = dist[trail][a] = 1;
            dist[b][trail] = dist[trail][b] = 1;
            trail++;
        }
        //Floyd
        for (int k = 0; k < trail; k++){
            for (int i = 0; i < trail; i++){
                for (int j = 0; j < trail; j++){
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }
        //找到最小的最長路徑
        int ans = inf;
        for (int i = 0; i < trail; i++){
            sort(dist[i], dist[i] + n);
            ans = min(ans, dist[i][n - 1]);
        }
        //輸出答案
        cout << "Case #" << c << ":\n"
             << ans << "\n\n";
    }
}
```