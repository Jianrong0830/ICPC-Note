---
tags: UVA
---
# UVA 12376 - As Long as I Learn, I Live
#DFS #Greedy

## 想法
用Greedy，每次都走價值最大的即可

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int t;
int n, m;
int a[105];
int map[105][105];
int visited[105];
int ans_a = 0, ans_id = 0;

void dfs(int id){
    ans_a += a[id];
    ans_id = id;
    visited[id] = 1;
    int max_a = 0, max_id = 0;
    for (int i = 0; i < n; i++){
        if(map[id][i]==1 && visited[i]==0 && a[i]>max_a){
            max_a = a[i];
            max_id = i;
        }
    }
    if (max_a == 0)
        return;
    dfs(max_id);
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    for (int k = 1; k <= t; k++){
        //初始化
        clr(map); clr(visited); clr(a);
        ans_a = 0, ans_id = 0;
        //輸入測資
        cin >> n >> m;
        for (int i = 0; i < n; i++)
            cin >> a[i];
        while(m--){
            int u, v;
            cin >> u >> v;
            map[u][v] = 1;
        }
        //執行DFS搜尋
        dfs(0);
        //輸出答案
        cout << "Case " << k << ": " << ans_a << ' ' << ans_id << '\n';
    }
}
```