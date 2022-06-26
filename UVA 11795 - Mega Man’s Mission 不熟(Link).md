---
tags: UVA
---
# UVA 11795 - Mega Man's Mission 不熟([Link](https://onlinejudge.org/external/117/11795.pdf))
#TSP

## 想法
先把每個武器所能打倒的敵人，以二進位的方式存下，編號小的存在 LSB
dp[N] 是當前集合的次數；state[N] 是當前能擊敗的集合
用all存所有的狀態並遍歷一遍
每次會判斷當前需要擊倒且可以擊倒的，ok = (~i)&state[i]
接著選擇要擊倒哪個機器人r，if(ok&r)則擊倒
state[r|i]是原本的加上(or)擊倒第二個後所獲得之武器能對付的(state[r | i] = state[i] | robot[j])
答案要求的集合是已全部擊倒的，dp[all - 1]

## Code
```c=
#include <iostream>
#include <string.h>
#include <algorithm>
#define MAXN 1<<16
#define int long long
#define clr(k) memset(k, 0, sizeof(k));
using namespace std;

int t;
int n, i, j;
int dp[MAXN];//當前集合的次數
int state[MAXN];//當前能擊敗的集合

int32_t main(void){
	//輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    char str[17];
	cin >> t;
	for (int c = 1; c <= t; c++){
		//初始化
		clr(dp); clr(state);
        int robot[17] = {};		//0 為 mega buster,其餘為擊敗此 robot[] 後，可以打倒的其他 robot 集合
		//輸入測資
		cin >> n;
        fill(state, state + (1 << n), 0);
        fill(dp, dp + (1 << n), 0);

        for (i = 0; i <= n; i++){
            gets(str);
            //讓第一個存在 LSB
            int r = 1;
            for (j = 0; j < n; j++){
                if (str[j] == '1')
                    robot[i] |= r;
                r <<= 1;
            }
        }
		//初始化
        state[0] = robot[0];
        dp[0] = 1;
        int all = 1 << n;//全部的組合
        for (i = 0; i < all; i++){
            int ok = (~i)&state[i];		//未走且可走的
            //選擇要擊倒哪個
            int r = 1;
            for (j = 1; j <= n; j++){
                if (ok&r){
                    dp[r | i] += dp[i];
                    //擊倒完後的 state 集合 ，變成 "未擊倒前的" 加上(or) "擊倒後才可以的"
                    state[r | i] = state[i] | robot[j];
                }
                r <<= 1;	//next
            }
        }
		//輸出答案
		cout << "Case " << c << ": " << dp[all - 1] << '\n';
	}
}
```
