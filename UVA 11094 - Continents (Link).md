---
tags: UVA
---
# UVA 11094 - Continents ([Link](https://onlinejudge.org/external/110/11094.pdf))
#Flood Fill

## 想法
雙迴圈遍歷每個點，若還沒走過and是陸地就執行DFS搜尋，並記錄本次搜尋的陸地面積
答案為走訪過的最大陸地面積
注意：不是只有l和w，而是任2個符號，所以要自行判斷哪個是陸地

## Code
```c=
#include <iostream>
#include <string.h>
#include <vector>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int m, n;
char map[25][25];
int visited[25][25];
string s;
int x, y;
int ans, temp;
char land;

void dfs(int x, int y){
    visited[x][y] = 1;
    temp++;     //紀錄本次搜尋的面積
    //cout << "visiting: " << x << ' ' << y << endl;
    //往上
    if(x>0 && visited[x-1][y]==0 && map[x-1][y]==land){
        dfs(x - 1, y);
    }
    //往下
    if(x<m-1 && visited[x+1][y]==0 && map[x+1][y]==land){
        dfs(x + 1, y);
    }
    //往右
    if(y>0 && visited[x][y-1]==0 && map[x][y-1]==land){
        dfs(x, y - 1);
    }
    //往左
    if(y<n-1 && visited[x][y+1]==0 && map[x][y+1]==land){
        dfs(x, y + 1);
    }
    //在最左邊跳到最右邊
    if(y==0 && visited[x][n-1]==0 && map[x][n-1]==land){
        dfs(x, n - 1);
    }
    //在最右邊跳到最左邊
    if(y==n-1 && visited[x][0]==0 && map[x][0]==land){
        dfs(x, 0);
    }
}

int32_t main(void){
    //輸入輸出優化
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>m>>n){
        //初始化
        clr(map); clr(visited);
        ans = temp = 0;
        //輸入測資
        for (int i = 0; i < m; i++){
            cin >> s;
            for (int j = 0; j < n; j++){
                map[i][j] = s[j];
            }
        }
        cin >> x >> y;
        cin.get();
        land = map[x][y];       //紀錄陸地的符號
        dfs(x, y);
        //遍歷每個點，若還沒走過and是陸地就執行DFS搜尋，並記錄本次搜尋的陸地面積及更新答案
        for (int i = 0; i < m; i++){
            for(int j=0; j<n; j++){
                if(visited[i][j]==0 && map[i][j]==land){
                    temp = 0;
                    dfs(i, j);
                    ans = max(ans, temp);
                }
            }
        }
        //輸出答案
        cout << ans << '\n';
    }
}
```