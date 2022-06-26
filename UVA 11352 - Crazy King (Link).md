---
tags: UVA
---
# UVA 11352 - Crazy King ([Link](https://onlinejudge.org/external/113/11352.pdf))
#BFS

## 
用BFS走訪一遍，找到A到B王國的最短距離
不過要先把馬和馬可以走到的點標記起來(visited[x][y]=1)，國王BFS走訪時不能經過
若最後B王國都沒有被走訪，代表國王沒辦法過去

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define MAXN 100
#define INF 0x3f3f3f
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int t;
int m, n;
int ax, ay;
int bx, by;
int visited[MAXN][MAXN], dist[MAXN][MAXN];
vector<pair<int, int>> horses;
int hdx[] = {1, 1, 2, 2, -1, -1, -2, -2};
int hdy[] = {2, -2, 1, -1, 2, -2, 1, -1};
int kdx[] = {-1, -1, -1, 0, 1, 1, 1, 0};
int kdy[] = {1, 0, -1, -1, -1, 0, 1, 1};

//判斷此點是否在棋盤內
int inBoard(int x, int y){
    if(x>=0 && x<m && y>=0 && y<n)
        return true;
    return false;
}

//國王的BFS走訪，紀錄國王到每點的最短距離
void BFS(int rx, int ry){
    deque<pair<int, int>> q;
    q.push_back({rx, ry});
    visited[rx][ry] = 1;
    while(!q.empty()){
        int x = q.front().first, y = q.front().second;
        q.pop_front();
        for (int i = 0; i < 8; i++){
            int tx = x + kdx[i], ty = y + kdy[i];
            if(inBoard(tx, ty) && !visited[tx][ty]){
                q.push_back({tx, ty});
                visited[tx][ty] = 1;
                dist[tx][ty] = dist[x][y] + 1;
            }
        }
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    while(t--){
        //初始化
        clr(visited); clr(dist);
        horses.clear();
        //輸入測資
        cin >> m >> n;
        string s;
        for (int i = 0; i < m; i++){
            cin >> s;
            for (int j = 0; j < n; j++){
                char c = s[j];
                if(c=='Z'){
                    visited[i][j] = 1;
                    dist[i][j] = INF;
                    horses.push_back({i, j});
                }
                if(c=='A')
                    ax = i, ay = j;
                if(c=='B')
                    bx = i, by = j;
            }
        }
        //紀錄馬可到達的位置
        for (int i = 0; i < horses.size(); i++){
            int x = horses[i].first, y = horses[i].second;
            for (int j = 0; j < 8; j++){
                int tx = x + hdx[j], ty = y + hdy[j];
                if(inBoard(tx,ty) && !(tx==ax && ty==ay) && !(tx==bx && ty==by)){
                    visited[tx][ty] = 1;
                    dist[tx][ty] = INF;
                }
            }
        }
        //執行算法
        BFS(ax, ay);
        //輸出答案
        int ans = dist[bx][by];
        if(!visited[bx][by])
            cout << "King Peter, you can't go now!\n";
        else
            cout << "Minimal possible length of a trip is " << ans << '\n';
        
    }
}
```