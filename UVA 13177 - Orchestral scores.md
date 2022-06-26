---
tags: UVA
---
# UVA 13177 - Orchestral scores
#二分搜
## 想法
把題目 x 個人一起合看一張樂譜 想成 
這個樂團最多 x 個人看一張樂譜，樂譜總數量有沒有超過 p
用二分搜尋找這個x及為答案

也可以用priority_queue

## Code
```c=
#include <iostream>
#include <string.h>
#include <math.h>
#define int long long
#define clr(k) memset(k, 0, sizeof(k));
using namespace std;

int p, n;
int x[100005];

//二分搜
//不另外開陣列紀錄i人一起看時需要多少樂譜，要用到再算(require)，不然會超時
int bs(int v, int l, int r){
    int m;
    while(l < r){
        int require = 0;
        m = (l + r) / 2;
        for (int i = 1; i <= n; i++){
            require += ceil(float(x[i]) / float(m));
        }
        if (v >= require){
            r = m;
        }
        else{
            l = m + 1;
        }
    }
    return r;
}

int32_t main(void){
    while(cin>>p>>n){
        clr(x);
        int m = 0;
        //二分搜的右邊界為最多人的團的人數
        for (int i = 1; i <= n; i++){
            cin >> x[i];
            m = max(m, x[i]);
        }
        cout << bs(p, 1, m) << '\n';
    }
}
```