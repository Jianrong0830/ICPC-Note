---
tags: UVA
---
# UVA 11957 - Checkers ([Link](https://onlinejudge.org/external/119/11957.pdf))
#DP

## 想法
就是基礎DP

## Code
```c=
#include <iostream>
#include <vector>
#include <algorithm>
#include <string.h>
#define int long long
#define inf 0x3f3f3f
#define clr(k) memset(k,0,sizeof(k))
#define MAXN 105
using namespace std;

int t;
int n;
int map[MAXN][MAXN];
int dp[MAXN][MAXN];

void init(){
    clr(dp);
    clr(map);
}

bool inBoard(int x, int y){
    if(x>=0 && x<n && y>=0 && y<n)
        return true;
    return false;
}

void DP(){
    for (int i = n - 1; i >= 0; i--){
        for (int j = 0; j < n; j++){
            if(map[i][j]==1){
                dp[i][j] = 1;
            }
            else if (map[i][j] != -1){
                if (inBoard(i + 1, j + 1) && map[i+1][j+1] != -1)
                    dp[i][j] += dp[i + 1][j + 1];
                else if(inBoard(i+2,j+2))
                    dp[i][j] += dp[i + 2][j + 2];
                if (inBoard(i + 1, j - 1) && map[i+1][j-1] != -1)
                    dp[i][j] += dp[i + 1][j - 1];                    
                else if(inBoard(i+2,j-2))
                    dp[i][j] += dp[i + 2][j - 2];
            }      
            dp[i][j] = dp[i][j] % 1000007;
        }
    }
}

int32_t main(){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    for (int k = 1; k <= t; k++){
        //初始化
        init();
        //輸入測資
        cin >> n;
        for (int i = 0; i < n; i++){
            string s;
            cin >> s;
            for (int j = 0; j < n; j++){
                char temp = s[j];
                if(temp=='B')
                    map[i][j] = -1;
                if(temp=='W')
                    map[i][j] = 1;
            }
        }
        //執行算法
        DP();
        int ans = 0;
        for (int i = 0; i < n; i++)
            ans = (ans + dp[0][i]) % 1000007;
        //輸出答案
        cout << "Case " << k << ": " << ans << '\n';
    }
}
```