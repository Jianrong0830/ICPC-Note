---
tags: UVA
---
# UVA 13095 - Tobby and Query
#DP

## 想法
先用DP記錄每個以0為起點的區段有幾個某數
dp[x]][y]的意思是：從0到x區間有幾個y，也就是複雜一點的cnt
tip:區間[l,r]=[0,r]-[0,l-1]
之後在目標區間[l,r]中，針對每個數i(0~9)，看他的cnt(dp[r][i]-dp[l-1][i])有沒有>0
大於0代表此數字有出現過，ans++

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define maxn 100020

using namespace std;

int n, q, l, r;
int dp[maxn][10];

int32_t main(void){
    while(cin>>n){
        memset(dp, 0, sizeof(dp));
        //先用DP記錄每個以0為起點的區段有幾個某數
        //dp[x]][y]的意思是：從0到x區間有幾個y，也就是複雜一點的cnt
        for (int i = 1; i <= n; i++){
            int temp;
            cin >> temp;
            dp[i][temp]++;
            for (int j = 0; j < 10; j++){
                dp[i][j] += dp[i - 1][j];
            }
        }
        cin >> q;
        while(q--){
            cin >> l >> r;
            int ans = 0;
            for (int i = 0; i < 10; i++){
                //[0,r]-[0,l-1]=[l,r]
                //若[l,r]內的i有出現過(cnt>0)，代表1種不同的數字，ans+1
                if(dp[r][i] - dp[l-1][i] > 0){
                    ans++;
                }
            }
            cout <<ans << '\n';
        }
    }
}
```