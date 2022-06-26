---
tags: UVA
---

# UVA 10203 - Snow Clearing ([Link](https://onlinejudge.org/external/102/10203.pdf))
#尤拉迴路

## 想法
尤拉迴路：每一條街繞一遍
找出尤拉迴路，算出迴路的長度並*2就是鏟雪車要走的距離
最後算花費時間即可

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define MAXN 1005
#define INF 0x3f3f3f3f3f3f
//#define int long long
using namespace std;

int t, n;
int a, b;
int vist[MAXN][MAXN];
int degree[MAXN];
vector<int> edge[MAXN];
vector<pair<int, int>> path;

void euler(int root){
    for(int it:edge[root]){
        if(!vist[root][it])
            continue;
        vist[root][it]--;
        vist[it][root]--;
        euler(it);
        path.push_back({root, it});
    }
}

int main(void){
    cin >> t;
    for(int k=1; k<=t; k++){
        //初始化
        memset(vist, 0, sizeof(vist));
        memset(degree, 0, sizeof(degree));
        for (int i = 1; i<=50; i++)
            edge[i].clear();
        path.clear();
        //輸入測資、建圖
        cin >> n;
        for(int i=1; i<=n; i++){
            cin >> a >> b;
            edge[a].push_back(b);
            edge[b].push_back(a);
            vist[a][b]++;
            vist[b][a]++;
            degree[a]++;
            degree[b]++;
        }
        //判斷尤拉迴路是否存在
        int flag = 0;
        for (int i = 1; i<=50; i++){
            if(degree[i]%2!=0){
                flag = 1;
                break;
            }
        }
        //不存在的輸出
        cout << "Case #" << k << '\n';
        if(flag){
            cout << "some beads may be lost\n\n";
            continue;
        }
        //存在則找出迴路並輸出
        euler(a);
        for(int i=path.size()-1; i>=0; i--){
            auto it = path[i];
            cout << it.first << " " << it.second << '\n';
        }
        cout << '\n';
    }
}
```