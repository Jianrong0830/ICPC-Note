---
tags: UVA
---
# UVA 10350 - 	Liftless EME ([Link](https://onlinejudge.org/external/103/10350.pdf))
#DP

## 想法
dp[下一層樓][k號樓梯] = min(dp[這層樓][k號樓梯] + cost[這層樓][到下一樓][經過 k 號樓梯],dp[下一層樓][k號樓梯]

## Code
```c=
#include <iostream>
#include <vector>
#include <algorithm>
#include <string.h>
#define int long long
#define inf 0x3f3f3f
#define clr(k) memset(k,0,sizeof(k))
#define MAXN 150
using namespace std;

string s;
int n, m;
int dp[MAXN][20];
int cost[MAXN][20][20];

void init(){
    memset(dp, inf, sizeof(dp));
    for (int i = 0; i < 20; i++)
        dp[1][i] = 0;
    clr(cost);
}

int32_t main(){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>s){
        //初始化
        init();
        //輸入測資
        cin >> n >> m;
        for(int i = 1; i < n; i++){
            for(int j = 1; j <= m; j++){
                for(int k = 1; k <= m; k++){
                    cin >> cost[i][j][k]; //cost[這層樓][到下一樓][經過 k 號樓梯]
                }
            }
        }
        //執行算法
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= m; j++){
                for (int k = 1; k <= m; k++){
                    dp[i + 1][k] = min(dp[i][j] + cost[i][j][k], dp[i + 1][k]);
                }
            }
        }
        //找出答案
        int ans = inf;
        for (int i = 1; i <= m; i++)
            ans = min(ans, dp[n][i]);
        //輸出答案
        cout << s << '\n';
        cout << ans + 2 * (n - 1) << '\n';
    }
}
```
