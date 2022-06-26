---
tags: UVA
---
# UVA 12841 - In Puzzleland (III) ([Link](https://vj.csgrandeur.cn/d5855bf8af09ae43f810dda4530a63e9?v=1644179522)) 不熟
#TSP

## 想法
用DFS走訪，選擇下一步的條件：在滿足可走完全部的情況下字典序最小的點
因此需用另一個走訪來檢查

## Code
```c=
#include <string.h>
#include <iostream>
#include <vector> 
#include <queue>
#include <algorithm>
using namespace std;

vector<int> g[26];
char path[32];
int n, m, st, ed, visited[26];
int bvisited[26] = {}, tvisited = 0;

int bfs(int u, int comp) {
    tvisited++;
    int cnt = 0, v;
    queue<int> Q;
    Q.push(u), bvisited[u] = tvisited;
    while (!Q.empty()) {
        u = Q.front(), Q.pop();
        cnt++;
        for (int i = 0; i < g[u].size(); i++) {
            v = g[u][i];
            if (bvisited[v] != tvisited && visited[v] == 0) {
                bvisited[v] = tvisited;
                Q.push(v);
            }
        }
    }
    return cnt == comp;
}
int dfs(int idx, int u) {
    path[idx] = u + 'A';
    if (idx < n - 1 && visited[ed])	return 0;
    if (idx == n-1) {
        path[n] = '\0';
        puts(path);
        return 1;
    }
    for (int i = 0; i < g[u].size(); i++) {
        int v = g[u][i];
        if (visited[v] || !bfs(v, n - idx - 1))	
            continue;
        visited[v] = 1;
        if(dfs(idx+1, v))	return 1;
        visited[v] = 0;
    }
    return 0;
}
int main() {
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);
    int testcase, cases = 0;
    int x, y;
    cin >> testcase;
    while (testcase--) {
        //輸入測資及初始化
        cin >> n >> m;
        char name[26][4], s1[4], s2[4], buf[26];
        int mg[26][26] = {};
        for (int i = 0; i < 26; i++)
            g[i].clear();
        for (int i = 0; i < n; i++)
            cin >> name[i];
        for (int i = 0; i < m; i++) {
            cin >> s1 >> s2;
            x = s1[0] - 'A', y = s2[0] - 'A';
            mg[x][y] = mg[y][x] = 1;
        }
        st = name[0][0] - 'A', ed = name[n-1][0] - 'A';
        for (int i = 0; i < 26; i++) {
            for (int j = 0; j < 26; j++) {
                if (mg[i][j])
                    g[i].push_back(j);
            }
        }
        //輸出測資
        cout << "Case " << ++cases << ": ";
        memset(visited, 0, sizeof(visited));
        visited[st] = 1;
        int f = dfs(0, st);
        if (!f)	puts("impossible");
    }
}
```