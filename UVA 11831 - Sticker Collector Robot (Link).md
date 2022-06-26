---
tags: UVA
---
# UVA 11831 - Sticker Collector Robot ([Link](https://onlinejudge.org/external/118/11831.pdf))
#水題

## 想法
純粹模擬，先將每個方向轉向後的結果記錄起來，方便之後使用
記得拿完貼紙後要刪掉

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int n, m, s;
char turn[100][100];
int map[105][105];
string command;
int start_x, start_y;
char start_d;

void init(){
    clr(map);
    turn[(int)'N'][(int)'D'] = 'L';
    turn[(int)'N'][(int)'E'] = 'O';
    turn[(int)'S'][(int)'D'] = 'O';
    turn[(int)'S'][(int)'E'] = 'L';
    turn[(int)'L'][(int)'D'] = 'S';
    turn[(int)'L'][(int)'E'] = 'N';
    turn[(int)'O'][(int)'D'] = 'N';
    turn[(int)'O'][(int)'E'] = 'S';
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    while(cin>>n>>m>>s && n){
        //初始化
        init();
        //輸入測資
        for (int i = 0; i < n; i++){
            string s;
            cin >> s;
            for (int j = 0; j<m; j++){
                if(s[j]=='*')
                    map[i][j] = 1;
                if(s[j]=='#')
                    map[i][j] = -1;
                if(s[j]=='N' || s[j]=='S' || s[j]=='L' || s[j]=='O'){
                    start_x = i, start_y = j, start_d = s[j];
                }
            }
        }
        cin >> command;
        //執行模擬
        int x = start_x, y = start_y;
        char d = start_d;
        int ans = 0;
        for(char c:command){
            //前進
            if(c=='F'){
                if(d=='N' && x>0 && map[x-1][y]!=-1)
                    x--;
                if(d=='S' && x<n-1 && map[x+1][y]!=-1)
                    x++;
                if(d=='O' && y>0 && map[x][y-1]!=-1)
                    y--;
                if(d=='L' && y<m-1 && map[x][y+1]!=-1)
                    y++;
            }
            //右轉 or 左轉
            else
                d = turn[(int)d][(int)c];
            if(map[x][y]==1){
                ans++;
                map[x][y] = 0;
            }
        }
        //輸出答案
        cout << ans << '\n';
    }
}
```