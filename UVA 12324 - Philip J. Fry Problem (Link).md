---
tags: UVA
---
# UVA 12324 - Philip J. Fry Problem ([Link](https://))
#DP

## 想法
稍微難想一點的DP，dp[i][j] = 已經走到i，有 j 個暗物質的最少時間
使用temp = min(j + b[i], n) 而不是j
轉移式分成兩種情況：
1.沒有使用暗物質 dp[i][temp] = min(dp[i][temp], dp[i - 1][j] + t[i])
2.使用暗物質 dp[i][temp - 1] = min(dp[i][temp - 1], dp[i - 1][j] + t[i] / 2)
最後在dp[n][0~n]中找到最小的值即為答案

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
#define INF 2147483647
using namespace std;

int n;
int t[105], b[105];
int dp[105][105];

int32_t main(void){
	//輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	while(cin>>n && n){
		//初始化
		clr(t); clr(b);
		for (int i = 0; i < 105; i++){
			for (int j = 0; j < 105; j++)
				dp[i][j] = INF;		//dp是用min算
		}
		dp[0][0] = 0;
		//輸入測資
		for (int i = 1; i <= n; i++){
			cin >> t[i] >> b[i];
		}
		//dp運算
		for (int i = 1; i <= n; i++){
			for (int j = 0; j <= n; j++){
				int temp = min(j + b[i], n);
				dp[i][temp] = min(dp[i][temp], dp[i - 1][j] + t[i]);		//沒有使用暗物質
				if(j){		//使用暗物質
					dp[i][temp - 1] = min(dp[i][temp - 1], dp[i - 1][j] + t[i] / 2);
				}
			}
		}
		//算出真正的答案
		int ans = INF;
		for (int i = 0; i <= n; i++){
			ans = min(ans, dp[n][i]);
		}
		cout << ans << '\n';
	}
}
```