---
tags: UVA
---
# UVA 11906 - Knight in a War Grid
#DFS

## 想法
用DFS從(0,0)走訪一遍，走訪到就+1(注意(0,0)要另外加，因為它是起點，DFS加不到)
DFS的終止：此節點之前就被走訪過(>0)，再走訪就是多餘的了
最後再算這些點有幾個是奇數，幾個是偶數就是答案。

## Code
```c=
#include <iostream>
#include <vector>
#define int long long
using namespace std;

int t;
int r, c, m, n;
vector<vector<int>> map;
int w;
vector<pair<int, int>> movement;		//紀錄8種可能的移動方式

void dfs(int x, int y){
	map[x][y]++;		//走訪到就+1
	if(map[x][y]>1)		//以前已有被走訪就退出，因為之前已經完成後面的步驟了(>1而不是>0是因為判斷時已加上當次的走訪)
		return;
	//搜尋可能的移動位置
	for (int i = 0; i < 8; ++i){
		int nx = x + movement[i].first, ny = y + movement[i].second;
		if(nx >= 0 && ny >= 0 && nx < r && ny < c && map[nx][ny] != -1)
			dfs(nx, ny);
		//m=0 or n=0，只要搜尋前三個，後面是重複的
		if(i == 3 && (!m || !n))
			break;
		//m=n，只要搜尋第1、2、5、6個，其他是重複的
		if(i == 1 && m == n)
			i += 4;
	}
}

int32_t main(void){
	//輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	cin >> t;
	for (int k = 1; k <= t; k++){
		//輸入測資
		cin >> r >> c >> m >> n;
		cin >> w;
		
		//初始化
		map.clear();
		map.assign(r, vector<int>(c));
		movement.clear();

		//紀錄8種移動方式，經特別排列，以便跳過重複的移動
		movement.push_back(make_pair(m, n));
		movement.push_back(make_pair(-m, -n));
		movement.push_back(make_pair(n, m));
		movement.push_back(make_pair(-n, -m));
		movement.push_back(make_pair(-n, m));
		movement.push_back(make_pair(n, -m));
		movement.push_back(make_pair(-m, n));
		movement.push_back(make_pair(m, -n));

		//標記有水的地方
		while(w--){
			int a, b;
			cin >> a >> b;
			map[a][b] = -1;
		}

		//執行DFS
		dfs(0, 0);
		++map[0][0];

		//計算even和odd
		int even = 0, odd = 0;
		for (int x = 0; x < r; x++){
			for (int y = 0; y < c; y++){
				if(map[x][y] > 0){
					if(map[x][y] % 2 == 0)
						even++;
					else
						odd++;
				}
			}
		}

		//輸出答案
		cout << "Case " << k << ": " << even << ' ' << odd << '\n';
	}
}
```