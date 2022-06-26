---
tags: UVA
---
# UVA 11790 - Murcia's Skyline
#DP
## 想法
其實不用用到LIS和LDS，就只是DP而已
若h[i]可以接在h[j]後面且接上去長度比不接大，
則更新LIS[i]、LDS[i] (考慮到第i項的最大寬度)

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k, 0, sizeof(k))
using namespace std;

int t, n;
int h[10005], w[10005];
int LIS[10005], LDS[10005];

int32_t main(void){
    cin >> t;
    for (int k = 1; k <= t; k++){
        clr(h); clr(w); clr(LIS); clr(LDS);
        cin >> n;
        for (int i = 1; i <= n; i++){
            cin >> h[i];
        }
        for (int i = 1; i <= n; i++){
            cin >> w[i];
            //LIS、LDS要初始化為width[i]，因為就算只選他自己也會有這個寬度
            LIS[i] = w[i];
            LDS[i] = w[i];
        }
        int mxLIS = 0, mxLDS = 0;
        //若h[i]可以接在h[j]後面且接上去長度比不接大，則更新LIS[i]、LDS[i](考慮到第i項的最大寬度)
        for (int i = 1; i <= n; i++){
            for (int j = 1; j < i; j++){
                if(h[i] > h[j]){
                    LIS[i] = max(LIS[i], LIS[j] + w[i]);
                }
                if(h[i] < h[j]){
                    LDS[i] = max(LDS[i], LDS[j] + w[i]);
                }
            }
            //同時更新答案
            mxLIS = max(mxLIS, LIS[i]);
            mxLDS = max(mxLDS, LDS[i]);
        }
        if(mxLIS >= mxLDS){
            cout << "Case " << k << ". Increasing (" << mxLIS << "). Decreasing (" << mxLDS << ")." << '\n';
        }
        else{
            cout << "Case " << k << ". Decreasing (" << mxLDS << "). Increasing (" << mxLIS << ")." << '\n';
        }
    }
}
```