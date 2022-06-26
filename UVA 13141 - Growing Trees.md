---
tags: UVA
---
# UVA 13141 - Growing Trees
#DP #費氏數列
## 想法
level i 的樹幹數量就是費氏數列的第i項
記得long long，不然會WA

## Code
```c=
#include <iostream>
#define int long long
using namespace std;

int n;
int dp[86];

int32_t main(void){
    //建立費氏數列
    dp[0] = 0, dp[1] = 1;
    for (int i = 2; i <= 85; i++){
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    while(cin>>n && n!=0){
        //輸出答案
        cout << dp[n] << "\n";
    }
}
```