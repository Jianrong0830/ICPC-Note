---
tags: UVA
---
# UVA 11585 - Nurikabe ([Link](https://onlinejudge.org/external/115/11585.pdf))
#Flood Fill

## 想法
超級麻煩的Flood Fill
用DFS去執行第一、二、三個判斷
第四個判斷用雙迴圈就好
注意：能刪減的地方就刪減，且要檢查的點先用一個vector存起來，不然會TLE

## Code
```c=
#include <iostream>
#include <string.h>
#include <vector>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int t;
int r, c, d;
int map[105][105];
int visited[105][105];
vector<pair<int, int>> v;   //存取第三、四個判斷需用的座標

int dfs(int x, int y, int g){
    int area = 1;
    visited[x][y] = 1;
    //往上
    if(x>0 && visited[x-1][y]==0 && map[x-1][y]==g){
        area += dfs(x - 1, y, g);
    }
    //往下
    if(x<r-1 && visited[x+1][y]==0 && map[x+1][y]==g){
        area += dfs(x + 1, y, g);
    }
    //往左
    if(y>0 && visited[x][y-1]==0 && map[x][y-1]==g){
        area += dfs(x, y - 1, g);
    }
    //往右
    if(y<c-1 && visited[x][y+1]==0 && map[x][y+1]==g){
        area += dfs(x, y + 1, g);
    }
    return area;
}

int num;
int dfs2(int x, int y){
    int area = 1;
    visited[x][y] = 1;
    if(map[x][y]>0)
        num = map[x][y];
    //往上
    if(x>0 && visited[x-1][y]==0 && map[x-1][y]>=0){
        area += dfs2(x - 1, y);
    }
    //往下
    if(x<r-1 && visited[x+1][y]==0 && map[x+1][y]>=0){
        area += dfs2(x + 1, y);
    }
    //往左
    if(y>0 && visited[x][y-1]==0 && map[x][y-1]>=0){
        area += dfs2(x, y - 1);
    }
    //往右
    if(y<c-1 && visited[x][y+1]==0 && map[x][y+1]>=0){
        area += dfs2(x, y + 1);
    }
    return area;
}

int dfs3(int x, int y){
    visited[x][y] = 1;
    if(x==0 || y==0 || x==r-1 || y==c-1){
        return true;
    }
    //往上
    if(x>0 && visited[x-1][y]==0 && map[x-1][y]>=0 && dfs3(x-1,y)){
        return true;
    }
    //往下
    if(x<r-1 && visited[x+1][y]==0 && map[x+1][y]>=0 && dfs3(x+1,y)){
        return true;
    }
    //往左
    if(y>0 && visited[x][y-1]==0 && map[x][y-1]>=0 && dfs3(x,y-1)){
        return true;
    }
    //往右
    if(y<c-1 && visited[x][y+1]==0 && map[x][y+1]>=0 && dfs3(x,y+1)){
        return true;
    }
    //左上
    if(x>0 && y>=0 && visited[x-1][y-1]==0 && map[x-1][y-1]>=0 && dfs3(x-1,y-1)){
        return true;
    }
    //右上
    if(x>=0 && y<c-1 && visited[x-1][y+1]==0 && map[x-1][y+1]>=0 && dfs3(x-1,y+1)){
        return true;
    }
    //左下
    if(x<r-1 && y>0 && visited[x+1][y-1]==0 && map[x+1][y-1]>=0 && dfs3(x+1,y-1)){
        return true;
    }
    //右下
    if(x<r-1 && y<c-1 && visited[x+1][y+1]==0 && map[x+1][y+1]>=0 && dfs3(x+1,y+1)){
        return true;
    }
    return false;
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> t;
    while(t--){
        //cin.get();
        //初始化
        clr(map);
        v.clear();
        //輸入測資
        cin>>r>>c>>d;
        while(d--){
            int A, B, C;
            cin >> A >> B >> C;
            map[A][B] = C;
        }
        int shaded_cnt = 0;
        int sx = -1, sy = -1;
        for (int i = 0; i < r; i++){
            string s;
            cin >> s;
            for (int j = 0; j < c; j++){
                char temp = s[j];
                if(temp=='#'){
                    map[i][j] = -1;
                    shaded_cnt++;   //計算shaded space數量
                    sx = i, sy = j;
                }
                //存取第三、四個判斷需用的座標
                else if(map[i][j]>=0){
                    v.push_back(make_pair(i, j));
                }
            }
        }
        //為4個constraints建立bool
        bool b1 = 1, b2 = 1, b3 = 1, b4 = 1;
        //The first constraint
        clr(visited);
        if(sx>=0 && sy>=0 && dfs(sx,sy,-1)!=shaded_cnt){
            b1 = false;
        }
        //The second constraint
        for (int i = 0; i < v.size(); i++){
            clr(visited);
            num = 0;
            int x = v[i].first, y = v[i].second;
            if(visited[x][y]==0){
                int temp = dfs2(x, y);
                if(temp!=num){
                    b2 = false;
                    break;
                }
            }
        }
        // The third constraint
        for (int i = 0; i < v.size(); i++){
            clr(visited);
            int x = v[i].first, y = v[i].second;
            if(map[x][y]>=0 && dfs3(x,y)==0){
                b3 = false;
                break;
            }
        }
        //The forth constraint
        for (int i = 0; i < r - 1; i++){
            for (int j = 0; j < c - 1; j++){
                if(map[i][j]>=0)
                    continue;
                if(map[i][j+1]>=0)
                    continue;
                if(map[i+1][j]>=0)
                    continue;
                if(map[i+1][j+1]>=0)
                    continue;
                b4 = false;
                break;
            }
            if(b4==0)
                break;
        }
        //輸出答案
        if(b1 && b2 && b3 && b4){
            cout << "solved\n";
        }
        else{
            cout << "not solved\n";
        }
        //cout << b1 << ' ' << b2 << ' ' << b3 << ' ' << b4 << '\n'; Debug
    }
}
```
