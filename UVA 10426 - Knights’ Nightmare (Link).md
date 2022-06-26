---
tags: UVA
---
# UVA 10426 - Knights’ Nightmare ([Link](https://onlinejudge.org/external/104/10426.pdf))
#BFS

## 想法
先用BFS走訪，記錄一個騎士到每個點的距離(有考慮怪獸和不考慮怪獸個走一次，並分別存起來)
再算所有騎士在有考慮怪的情況下到某個點的總距離(cost)
有一個騎士可以經過怪獸沒關係
找到經過怪獸可以縮短最少距離的騎士並記錄減少的量(reduce)
再將cost-reduce即為四騎士到某點的最短距離
最後再找到四騎士到所有點的最短距離中最小的即為答案
若答案為INF代表無解

```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define MAXN 20
#define INF 0x3f3f3f
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int dx[] = {1, 1, 2, 2, -1, -1, -2, -2};
int dy[] = {2, -2, 1, -1, 2, -2, 1, -1};
int dist[4][MAXN][MAXN], dist2[4][MAXN][MAXN];
int visited[4][MAXN][MAXN], visited2[4][MAXN][MAXN];
string set;
int r, c;
vector<pair<int, int>> knights;
int mx,my;

//判斷此座標是否在棋盤內
int inBoard(int x, int y){
    if(x>0 && x<=r && y>0 && y<=c)
        return true;
    return false;
}

//用BFS走訪記錄一個騎士到每個點的距離(分有怪獸和沒怪獸的)
void BFS(int rx, int ry, int dist[MAXN][MAXN], int visited[MAXN][MAXN], int monster){
    deque<pair<int, int>> q;
    q.push_back(make_pair(rx, ry));
    visited[q.front().first][q.front().second] = 1;
    if(monster==1)
        visited[mx][my] = 1;
    int x, y, tx, ty;
    while(!q.empty()){
        x = q.front().first, y = q.front().second;
        q.pop_front();
        for (int i = 0; i < 8; i++){
            tx = x + dx[i], ty = y + dy[i];
            if(inBoard(tx,ty) && !visited[tx][ty]){
                dist[tx][ty] = dist[x][y] + 1;
                visited[tx][ty] = 1;
                q.push_back(make_pair(tx, ty));
            }
        }
    }
}

//算出一個點的total cost
int distance(int x, int y){
    if(x==mx && y==my)
        return INF;
    int cost = 0, reduce = 0;
    for (int i = 0; i < 4; i++){
        if(dist2[i][x][y]==0 && !visited2[i][x][y])
            return INF;
        cost += dist2[i][x][y];
    }
    for (int i = 0; i < 4; i++){
        if(dist[i][x][y]==0 && !visited[i][x][y])
            continue;
        reduce = max(reduce, dist2[i][x][y]-dist[i][x][y]);
    }
    return cost - reduce;
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>set && set!="0"){
        //初始化
        clr(dist); clr(dist2); clr(visited); clr(visited2);
        knights.clear();
        //輸入測資
        cin>>r>>c;
        for (int i = 0; i < 4; i++){
            int x, y;
            cin >> x >> y;
            knights.push_back(make_pair(x, y));
        }
        cin >> mx >> my;
        //執行算法
        for (int i = 0; i < 4; i++){
            BFS(knights[i].first, knights[i].second, dist[i], visited[i], 0);   //先不管怪獸
            BFS(knights[i].first, knights[i].second, dist2[i], visited2[i], 1); //再走一次有管怪獸的
        }
        int ans = INF;
        for (int i = 1; i <= r; i++){       //找出最小的total cost
            for (int j = 1; j <= c; j++){
                ans = min(ans, distance(i, j));
            }
        }
        //輸出答案
        cout << set << '\n';
        if(ans==INF)
            cout << "Meeting is impossible.\n";
        else
            cout << "Minimum time required is " << ans << " minutes.\n";
    }
}
```