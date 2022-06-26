---
tags: UVA
---
# UVA 10259 - Hippity Hopscotch ([Link](https://onlinejudge.org/external/102/10259.pdf))
#DP

## 想法
將所有數字排序，再進行DP

## Code
```c=
#include <iostream>
#include <stdio.h>
#include <algorithm>

using namespace std;
struct E {
    int i, j, v;
    bool operator<(const E &a) const {
        return v < a.v;
    }
};

int main() {
    E D[10005];
    int testcase;
    int i, j, k, x, y;
    int N, K;
    int g[105][105], dp[105][105];
    scanf("%d", &testcase);
    while(testcase--) {
        //輸入測資
        scanf("%d %d", &N, &K);
        int n = 0;
        for(i = 0; i < N; i++) {
            for(j = 0; j < N; j++) {
                scanf("%d", &g[i][j]);
                D[n].i = i, D[n].j = j;
                D[n].v = g[i][j];
                n++;
                dp[i][j] = 0;
            }
        }
        //執行算法
        sort(D, D+n);
        dp[0][0] = g[0][0];
        int can[105][105] = {};
        for(i = 0; i < n; i++) {
            if(D[i].i == 0 && D[i].j == 0)
                break;
        }
        can[0][0] = 1;
        for(; i < n; i++) {
            if(can[D[i].i][D[i].j] == 0)    continue;
            j = 1, x = D[i].i+1, y = D[i].j;
            while(x < N && j <= K) {
                if(g[x][y] > D[i].v) {
                    dp[x][y] = max(dp[x][y], dp[D[i].i][D[i].j]+g[x][y]);
                    can[x][y] = 1;
                }
                x++, j++;
            }
            j = 1, x = D[i].i, y = D[i].j+1;
            while(y < N && j <= K) {
                if(g[x][y] > D[i].v) {
                    dp[x][y] = max(dp[x][y], dp[D[i].i][D[i].j]+g[x][y]);
                    can[x][y] = 1;
                }
                y++, j++;
            }
            j = 1, x = D[i].i-1, y = D[i].j;
            while(x >= 0 && j <= K) {
                if(g[x][y] > D[i].v) {
                    dp[x][y] = max(dp[x][y], dp[D[i].i][D[i].j]+g[x][y]);
                    can[x][y] = 1;
                }
                x--, j++;
            }
            j = 1, x = D[i].i, y = D[i].j-1;
            while(y >= 0 && j <= K) {
                if(g[x][y] > D[i].v) {
                    dp[x][y] = max(dp[x][y], dp[D[i].i][D[i].j]+g[x][y]);
                    can[x][y] = 1;
                }
                y--, j++;
            }
        }

        int ret = 0;
        for(i = 0; i < N; i++) {
            for(j = 0; j < N; j++) {
                ret = max(ret, dp[i][j]);
            }
        }
        //輸出答案
        printf("%d\n", ret);
        if(testcase)    puts("");
    }
}
```