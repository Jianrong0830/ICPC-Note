---
tags: UVA
---
# UVA 11566 - Let's Yum Cha!
#DP #背包問題
## 想法
就是複雜版的背包問題
空間限制為2(n+1)，費用限制為為 x * (n + 1) / (float)1.1 - t*(n + 1)
每道菜最多點2次，直接都複製一份再來運算

## Code
```c=
#include <iostream>
#include <string.h>
#include <iomanip>
#define int long long
using namespace std;

int n, x, t, k;
//n：幾個朋友 x：每人最多能出的錢 t：每人喝茶的額外費用 k：幾道菜
int ans;
int dp[25][2010], price[210], value[210];

void solve(void){
    int budget = x * (n + 1) / (float)1.1 - t*(n + 1);
    ans = 0;
    //3層迴圈，但DP只有2維
    //第2、3層迴圈就是用第1層的price、value算的
    for (int i = 1; i <= k; i++){
        int p = price[i], v = value[i];
        //要從上界到下界
        for (int a = 2*(n + 1); a >0; a--){
            for (int b = budget; b > 0; b--){
                dp[a][b] = max(dp[a][b], dp[a - 1][b - p] + v);
                dp[a][b] = max(dp[a][b], dp[a - 1][b]);
            }
        }
    }
    //答案不是最後一個DP，而是其中的最大值
    for (int i = 1; i <= 2 * (n + 1); i++){
        for (int j = 1; j <= budget; j++){
            if(dp[i][j]>ans)
                ans = dp[i][j];
        }
    }
}

int32_t main(void){
    while(cin>>n>>x>>t>>k && (n||x||t||k)){
        memset(dp, 0, sizeof(dp));
        memset(price, 0, sizeof(price));
        memset(value, 0, sizeof(value));
        //value=所有人喜愛值得總和
        for (int i = 1; i <= k*2; i+=2){
            int sum = 0, temp;
            cin >> price[i];
            price[i + 1] = price[i];
            for (int j = 1; j <= n + 1; j++)
            {
                cin >> temp;
                sum += temp;
            }
            value[i] = sum, value[i + 1] = sum;
        }
        solve();
        cout << setprecision(2) << fixed << (double) ans / (n+1) << '\n';
    }
}
```