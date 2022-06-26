---
tags: UVA
---
# UVA 10505 - Montesco vs Capuleto
#DFS #塗色問題

## 想法
當成塗色問題就好(2色)
相鄰的點要塗成不同色，每個集合的哪個顏色多就邀請那個顏色的人
注意：
有可能矛盾(亦是朋友亦是敵人)，要將矛盾點標記成特別的顏色
最後再從這個顏色開始DFS，清空矛盾集合的人

## Code
```c=
#include <iostream>
#include <string.h>
#include <vector>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int t, n;
int map[205][205];
int color[205];
int visited[205];
vector<vector<int>> v;      //紀錄各個集合
vector<int> temp;           //暫時存取一個集合，再存到v內

//DFS走訪並塗色，當前(顏色)為1，下一次就是-1
void dfs(int id, int c){
    //若走訪過，需判斷顏色是否一致
    if(visited[id]){
        if(color[id]==c)
            return;
        //不一致代表矛盾，先將color標記為2，之後再處理
        else{
            color[id] = 2;
            return;
        }
    }
    temp.push_back(id);
    visited[id] = 1;
    color[id] = c;
    for (int i = 1; i <= n; i++){
        if(map[id][i]==1){
            dfs(i, -c);
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
        clr(map); clr(color);
        v.clear();
        //輸入測資
        cin >> n;
        for (int i = 1; i <= n; i++){
            int p;
            cin >> p;
            while(p--){
                int e;
                cin >> e;
                map[i][e] = 1;
                map[e][i] = 1;
            }
        }
        //每個點都用DFS走訪並塗色
        clr(visited);
        for (int i = 1; i <= n; i++){
            if(visited[i]==0){
                temp.clear();
                dfs(i, 1);
                v.push_back(temp);
            }
        }
        //處理矛盾的集合
        //若顏色為2，就再用DFS走訪一次，將集合中所有點的顏色都標記為0
        clr(visited);
        for (int i = 1; i <= n; i++){
            if(color[i]==2){
                dfs(i, 0);
            }
        }
        //計算答案
        int ans = 0;
        //有用的顏色標記為1或-1，若此集合1比較多則邀請標記為1的人，反之
        for (int i = 0; i < v.size(); i++){
            int cnta = 0, cntb = 0;
            for (int j = 0; j < v[i].size(); j++){
                if(color[v[i][j]]==1){
                    cnta++;
                }
                if(color[v[i][j]]==-1){
                    cntb++;
                }
            }
            ans += max(cnta, cntb);
        }
        //輸出答案
        cout<<ans<<'\n';
    }
}
```