---
tags: UVA
---

# UVA 12644 - Vocabulary ([Link](https://onlinejudge.org/external/126/12644.pdf))
#二分圖匹配

## 想法
先把字串輸贏建立成圖：Jack單字i贏Jill單字j，則i連向j(edge[i].push_back(j))
之後就是基本的二分圖匹配
[參考](https://web.ntnu.edu.tw/~algo/Matching.html#4)

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define MAXN 200005
#define INF 0x3f3f3f3f3f3f
//#define int long long
using namespace std;

int n, m;
int mx[MAXN], my[MAXN], vist[MAXN];
string jill[MAXN], jack[MAXN];
vector<int> edge[MAXN];

//判斷當前x能否與y匹配
int dfs(int x){
    for(int y:edge[x]){
        if(!vist[y]){
            vist[y] = 1;
            if(my[y]==-1 || dfs(my[y])){
                mx[x] = y, my[y] = x;
                return true;
            }
        }
    }
    return false;
}

//計算最大匹配數
int bipartite_matching(){
    int ans = 0;
    for (int i = 1; i <= n; i++){
        memset(vist, 0, sizeof(vist));
        if(dfs(i))
            ans++;
    }
    return ans;
}

//判斷a單字使否與b單字連結(Jack贏Jill)
int connected(string a, string b){
    int cnta[30] = {0}, cntb[30] = {0};
    for (char i:a)
        cnta[int(i) - int('a')]++;
    for (char i:b)
        cntb[int(i) - int('a')]++;
    for (int i = 0; i < 26; i++){
        if(cnta[i]<cntb[i])
            return false;
    }
    return true;
}

int main(void){
    while(cin>>n>>m){
        //初始化
        memset(mx, -1, sizeof(mx));
        memset(my, -1, sizeof(my));
        for (int i = 0; i < MAXN; i++)
            edge[i].clear();
        //輸入測資
        for (int i = 1; i <= n; i++)
            cin >> jack[i];
        for (int i = 1; i <= m; i++)
            cin >> jill[i];
        //建圖
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= m; j++){
                if(connected(jack[i],jill[j]))
                    edge[i].push_back(j);
            }
        }
        //運算並輸出結果
        cout << bipartite_matching() << '\n';
    }
}
```