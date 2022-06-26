---
tags: UVA
---
# UVA 11420 - Chest of Drawers
#DP
## 想法
https://www.pinghenotes.com/UVa-11420-Chest-of-Drawers/
寫的超清楚
一開始的盲點：只用二維DP，沒有再把有沒有鎖看成一個維度
若原DP解起來還是很麻煩，甚至還要再跑for迴圈，可以嘗試再加一個維度

## Code
```c=
#include <iostream>
using namespace std;

int n, s;
long long dp[67][67][2];    //dp[i][j][k]：有i個抽屜，j個是安全的，第一格有沒有鎖(1代表有，0代表沒有)

int main(void){
    //輸入輸出加速
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    //特殊情況的初始化
    dp[1][0][0] = 1;
    dp[1][1][1] = 1;

    for (int i = 2; i <= 65; i++){
        //dp[i][0][0]算法不一樣，且不會有dp[i][0][1]的存在
        dp[i][0][0] = dp[i - 1][1][1] + dp[i - 1][0][0];
        //一般情況
        for (int j = 1; j <= i; j++){
            dp[i][j][0] = dp[i - 1][j + 1][1] + dp[i - 1][j][0];
            dp[i][j][1] = dp[i - 1][j - 1][1] + dp[i - 1][j - 1][0];
        }
    }
    //輸出答案
    while (cin >> n >> s , (n >= 0 && s >= 0)){
        cout << dp[n][s][0] + dp[n][s][1] << '\n';
    }
}
```