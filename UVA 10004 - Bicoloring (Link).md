---
tags: UVA
---
# UVA 10004 - Bicoloring ([Link](https://onlinejudge.org/external/100/10004.pdf))
#DFS
## 想法
用DFS去走訪每個節點，每個節點都圖上與上個節點不同的顏色
顏色用1,-1表示
每當完成一個節點，就檢查相鄰節點有沒有顏色一樣的，有的話代表無法達成，退出DFS
若所有節點都沒有顏色衝突的話就代表可達成

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int map[250][250];      //鄰接陣列
int visited[250];
int mark[250];          //記錄節點的顏色
int n, m;
string ans;

//用DFS去走訪每個節點，並塗上顏色
void dfs(int id, int color){
    mark[id] = color;
    visited[id] = 1;
    //塗完顏色就檢查相鄰節點有沒有顏色一樣的，有的話代表無法達成，直接退出
    for (int i = 0; i < n; i++){
        if(map[id][i]==1 && mark[i]==mark[id]){
            ans = "NOT BICOLORABLE.";
            return;
        }
    }
    //搜尋相鄰節點
    for (int i = 0; i < n; i++){
        if(map[id][i]==1 && visited[i]==0){
            dfs(i, -color);
        }
    }
}

int32_t main(void){
    while(cin>>n && n){
        clr(map); clr(visited); clr(mark);
        ans = "BICOLORABLE.";
        cin >> m;
        //建立圖
        for (int i = 0; i < m; i++){
            int a, b;
            cin >> a >> b;
            map[a][b] = 1;
            map[b][a] = 1;
        }
        dfs(0, 1);
        cout << ans << '\n';
    }
}
```