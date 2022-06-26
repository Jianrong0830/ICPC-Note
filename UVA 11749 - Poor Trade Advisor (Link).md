---
tags: UVA
---
# UVA 11749 - Poor Trade Advisor ([Link](https://onlinejudge.org/external/117/11749.pdf))
#DFS

## 想法
題目要求最大平均ppa，那這個值一定等於最大的ppa(只取那一條路即可得到)
以每個點為起點都執行一次DFS，若路徑=最大ppa則進行走訪，並用cnt記錄此次DFS走訪的點的數量
答案即為最大的cnt

注意：
    1.若cnt=1，代表只有起點被算到，有2種可能：
        a.周圍沒有max_ppa的路徑(不可能屬於province的一部份)，cnt其實是0才對
        b.周圍有max_ppa的路徑但之前已經被走過(不用再算一次)，那就把cnt歸零(不做也沒差)
        因此需要把cnt歸零
    2.ppa可能為負，因此map和max_ppa初始化為-2147483648
## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define nINF -2147483648
using namespace std;

int n, m;
int map[505][505];
int visited[505];
int cnt, max_ppa, ans;

void dfs(int id){
	cnt++;
	visited[id] = 1;
	for (int i = 1; i <= n; i++){
		if(!visited[i] && map[id][i]==max_ppa){		//若ppa=max_ppa才進行搜尋
			dfs(i);
		}
	}
}

int32_t main(void){
	//輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	while(cin>>n>>m && n){
		//初始化(注意：因為ppa可為負，因此map和max_ppa要初始化成nINF)
		memset(visited, 0, sizeof(visited));
		for (int i = 1; i <= n; i++){
			for (int j = 1; j <= n; j++)
				map[i][j] = nINF;
		}
		max_ppa = nINF, ans = 0;	
		//紀錄路徑的ppa
		while(m--){
			int a, b, c;
			cin >> a >> b >> c;
			map[a][b] = map[b][a] = max(map[a][b], c);		//兩點間可能有多條路徑，選ppa最大的紀錄
			max_ppa = max(max_ppa, c);		//順便記錄最大的ppa
		}
		//每個點都執行一次DFS
		for (int i = 1; i <= n; i++){
			cnt = 0;	//歸零
			dfs(i);
			//若cnt=1，代表只有起點被算到周圍有max_ppa的路徑但之前已經被走過(不用再算一次)，那就把cnt歸零(不做也沒差)，可能是周圍沒有max_ppa的路徑(不可能屬於province的一部份)，cnt其實是0才對
			//也有可能
			if(cnt==1)
				cnt = 0;
			//答案取最大的cnt
			ans = max(ans, cnt);
		}
		//輸出答案
		cout << ans << '\n';
	}
}
```