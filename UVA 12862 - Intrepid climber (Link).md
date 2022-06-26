---
tags: UVA
---
# UVA 12862 - Intrepid climber ([Link](https://onlinejudge.org/external/128/12862.pdf))
#DFS

## 想法
不斷的下山上山又下山，最後那一段不需要再上山花費體力，直接下山就好
因此將最花體力的那一段視為最後一段即可
將從山頂開始所有通道朋友的路徑加起來，再減去從山頂到朋友需花費最多體力的那一段就是答案
![](https://i.imgur.com/kt50y6L.jpg)
如上圖：
紅色路徑(6)減去藍色路徑(4)即為應輸出的答案

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int n, f;
vector<pair<int, int>> nodes[100005];		//nodes[a][b].first = a連到的節點；second = a到此節點的weight
int isFriend[100005];						//紀錄是否為朋友
int ans, max_weight;

int dfs(int id, int weights){		//回傳值為：從id往下可以找到幾個朋友；weights = 從top到id的weight
	int fcnt = 0;			//找到的朋友數
	if(isFriend[id]){		//若id就是朋友
		//更新max_weight及fcnt
		max_weight = max(max_weight, weights);
		fcnt++;
	}
	//開始一直找朋友
	for (int i = 0; i < nodes[id].size(); i++){
		int node = nodes[id][i].first, weight = nodes[id][i].second;
		int fcnt_tmp = dfs(node, weights + weight);		//從此node找到的朋友數
		fcnt += fcnt_tmp;		//更新fcnt
		if(fcnt_tmp)			//若有朋友，此weight要加到ans(總路徑)
			ans += weight;
	}
	return fcnt;
}

int32_t main(void){
	//輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	while(cin>>n>>f){
		//初始化
		for (int i = 0; i <= n; i++)
			nodes[i].clear();
		clr(isFriend);
		ans = max_weight = 0;
		//輸入測資
		for (int i = 0; i < n - 1; i++){
			int a, b, c;
			cin >> a >> b >> c;
			nodes[a].push_back(make_pair(b, c));
		}
		for (int i = 1; i <= f; i++){
			int temp;
			cin >> temp;
			isFriend[temp] = 1;
		}
		//執行搜尋
		dfs(1, 0);
		//輸出答案
		cout << ans - max_weight << '\n';
	}
}
```