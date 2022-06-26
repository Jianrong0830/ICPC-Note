---
tags: UVA
---
# UVA 11709	- Trust groups ([Link](https://onlinejudge.org/external/117/11709.pdf))
#Tarjan

## 想法
基礎Tarjan應用

## Code
```c=
#include <cstdio>
#include <iostream>
#include <map>
#include <vector>
#include <algorithm>
#define MAXN 1005
using namespace std;

vector<int> g[MAXN];
int vfind[MAXN], findIdx;
int stk[MAXN], stkIdx;
int in_stk[MAXN], visited[MAXN];
int scc_cnt;

int tarjan(int id) {
    in_stk[id] = visited[id] = 1;
    stk[++stkIdx] = id;
    vfind[id] = ++findIdx;
    int mn = vfind[id];
    for(vector<int>::iterator it = g[id].begin();
        it != g[id].end(); it++) {
        if(!visited[*it])
            mn = min(mn, tarjan(*it));
        if(in_stk[*it])
            mn = min(mn, vfind[*it]);
    }
    if(mn == vfind[id]) {
        do {
            in_stk[stk[stkIdx]] = 0;
        } while(stk[stkIdx--] != id);
        scc_cnt++;
    }
    return mn;
}

int main() {
    int n, m, x, y, i;
    char name[50], a[50], b[50];
    //輸入測資
    while(scanf("%d %d", &n, &m) == 2) {
        if(n == 0 && m == 0)
            break;
        map<string, int> mapped;
        getchar();
        for(i = 0; i < n; i++) {
            gets(name);
            mapped[name] = i;
            g[i].clear();
            visited[i] = in_stk[i] = 0;
        }
        for(i = 0; i < m; i++) {
            gets(a);
            gets(b);
            x = mapped[a];
            y = mapped[b];
            g[x].push_back(y);
        }
        //執行算法
        scc_cnt = 0;
        for(i = 0; i < n; i++) {
            if(!visited[i]) {
                findIdx = stkIdx = 0;
                tarjan(i);
            }
        }
        //輸出答案
        cout << scc_cnt << '\n';
    }
}

```