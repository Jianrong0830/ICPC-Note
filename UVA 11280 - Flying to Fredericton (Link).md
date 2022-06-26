---
tags: UVA
---
# UVA 11280 - Flying to Fredericton ([Link](https://onlinejudge.org/external/112/11280.pdf))
#DP #Dijsktra

## 想法
可以用DP，dp[i][j]=走i步到j的最短距離
轉移式：dp[t + 1][v] = min(dp[t + 1][v], dp[t][u] + c)，u為一邊的起點，v為終點

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

int T;
int n, m;
string city[MAXN];
vector<pair<int, int>> edge[MAXN];
int dp[MAXN][MAXN];
int q[12];

void init(){
    for (int i = 1; i <= n; i++){
        city[i] = " ";
        edge[i].clear();
    }
    memset(dp, inf, sizeof(dp));
    memset(q, 0, sizeof(q));
}

int id(string s){
    for (int i = 1; i <= n; i++){
        if(s==city[i])
            return i;
    }
    return 0;
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> T;
    for (int k = 1; k <= T; k++){
        init();     //初始化
        //輸入測資&建圖
        cin >> n;
        for (int i = 1; i <= n; i++)
            cin >> city[i];
        cin >> m;
        string a, b; int c;
        while(m--){
            cin >> a >> b >> c;
            edge[id(a)].push_back({id(b), c});
            edge[id(b)].push_back({id(a), c});
        }
        for (int i = 1; i <= 10; i++){
            cin >> q[i];
            if(q[i]==0)
                break;
        }
        //執行算法(DP)
        dp[0][id("Calgary")] = 0;
        for (int t = 0; t <= n; t++){
            for (int u = 1; u <= n; u++){
                if(dp[t][u]<inf){
                    for(auto temp:edge[u]){
                        int v = temp.first, c = temp.second;
                        dp[t + 1][v] = min(dp[t + 1][v], dp[t][u] + c);
                    }
                }
            }
        }
        //輸出答案
        cout << "Scenario #" << k << '\n';
        for (int i = 1; i <= 10; i++){
            if(q[i]==0)
                break;
            int ans = inf;
            for (int j = 1; j <= q[i]; j++){
                ans = min(ans, dp[j][id("Fredericton")]);
            }
            if(ans==inf)
                cout << "No satisfactory flights\n";
            
            else
                cout << "Total cost of flight(s) is $" << ans << '\n';
        }
    }
}
```