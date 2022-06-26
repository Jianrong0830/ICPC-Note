---
tags: UVA
---
# UVA 10354 - Avoiding Your Boss ([Link](https://onlinejudge.org/external/103/10354.pdf))
#Floyd

## 想法
對老闆做一次floyd，之後記錄老闆可能經過的地方，自己也做一次floyd，若遇到老闆可能經過的地方，兩點距離則設為inf
老闆可能經過的地方：
若dist1[bh][i]+dist1[i][of]==dist1[bh][of]，則ban[i]=1

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

int p, r, bh, of, yh, m;
int dist1[MAXN][MAXN], dist2[MAXN][MAXN];
int ban[MAXN];

void init(){
    for (int i = 1; i <= p; i++){
        for (int j = 1; j <= p;j++){
            dist1[i][j] = inf;
            dist2[i][j] = inf;
        }
        dist1[i][i] = dist2[i][i] = 0;
        ban[i] = 0;
    }
}
//老闆
void floyd1(){
    for (int k = 1; k <= p; k++){
        for (int i = 1; i <= p; i++){
            for (int j = 1; j <= p; j++){
                if(dist1[i][j]>dist1[i][k]+dist1[k][j])
                    dist1[i][j] = dist1[i][k] + dist1[k][j];
            }
        }
    }
}
//自己
void floyd2(){
    for (int k = 1; k <= p; k++){
        if(ban[k]) continue;
        for (int i = 1; i <= p; i++){
            if(ban[k]) continue;
            for (int j = 1; j <= p; j++){
                if(ban[k]) continue;
                if(dist2[i][j]>dist2[i][k]+dist2[k][j])
                    dist2[i][j] = dist2[i][k] + dist2[k][j];
            }
        }
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while (cin >> p >> r >> bh >> of >> yh >> m){
        init();     //初始化
        //輸入測資
        int a, b, c;
        while(r--){
            cin >> a >> b >> c;
            dist1[a][b] = dist1[b][a] = c;
            dist2[a][b] = dist2[b][a] = c;
        }
        //執行算法
        floyd1();
        for (int i = 1; i <= p; i++){
            if(dist1[bh][i]+dist1[i][of]==dist1[bh][of])
                ban[i] = 1;
        }
        floyd2();
        //輸出答案
        if(dist2[yh][m]==inf || ban[yh] || ban[m])
            cout << "MISSION IMPOSSIBLE.\n";
        else
            cout << dist2[yh][m] << '\n';
    }
}
```