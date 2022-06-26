---
tags: UVA
---
# UVA 10765 - Doves and bombs([Link](https://vj.csgrandeur.cn/4462b7e63af0ebecbcff19f1a968efc8?v=1644688736))
#DFS
## 想法
用DFS走訪，並記錄：
    1.dfn[id] (DFS走訪順位)
    2.low[id] (id不從父子邊能追溯到的最前面(dfn最小)的點的dfn)
用cnt陣列記錄每一個點有多少節點將其視為割點
有兩種情況：
    1.u是根節點，若u的子節點數>1，則從第2個開始的子節點將u視為割點，cnt++(當child>=2)
    2.u不是跟節點，若dfn[u]<=low[v]，則v將u視為割點，cnt[u]++
輸出時還要將cnt+1(包括父節點)

原本用習慣的鄰接陣列，但似乎記憶體需求太大會Wrong Answer，還是改用Vector
這題還沒有很熟，要再複習

## Code
```c=
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector< vector<int> > map;
vector<int> dfn, low;
vector< pair<int, int> > cnt;
int deep;

int n, m;

//用DFS走訪，並記錄dfn、low
void dfs(int id, int par){
	int child = 0;
	dfn[id] = low[id] = ++deep;
	for(auto& i : map[id]){
        //若還沒被走訪
		if (!dfn[i]) {
			child++;
			dfs(i, id);
			low[id] = min(low[id], low[i]);
		}
        //已被走訪且不是當前父節點
		else if(i != par) {
            low[id] = min(low[id], dfn[i]);
            continue;
        }
        //根據割點定義
		if (low[i] >= dfn[id] && (par != -1 || child >= 2))
            cnt[id].second++;
    }
}

//排序用
int cmp(pair<int, int>& a, pair<int, int>& b){
	return a.second == b.second ? a.first < b.first : a.second > b.second;
}

int main(){
    //輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	while (cin >> n >> m && n) {
        //初始化
		map.assign(n, vector<int>());
		dfn.assign(n, 0);
		low.assign(n, 0);
		cnt.clear();
		for (int i = 0; i < n; ++i) 
            cnt.push_back(pair<int, int>(i, 0));
		deep = 0;

		//建立圖
		int a, b;
		while (cin >> a >> b, a != -1) {
			map[a].emplace_back(b);
			map[b].emplace_back(a);
		}

		//執行DFS
		for (int i = 0; i < n; i++) 
            if (!dfn[i]) 
                dfs(i, -1);

		//排序cnt
		sort(cnt.begin(), cnt.end(), cmp);

		for (int i = 0; i < m; i++)
            cout << cnt[i].first << " " << cnt[i].second + 1 << "\n";
        //注意格式，最後要換行
        cout << "\n";
	}
}
```
