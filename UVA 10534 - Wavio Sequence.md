---
tags: UVA
---
# UVA 10534 - Wavio Sequence
#LIS
## 想法
LIS和LDS各做一遍
p1[i]是最後一個數是k[i]時他的LIS長度
p2[i]是第一個數是k[i]時他的LDS長度
從頭遍歷一遍，找出k[i]使得min(p1[i] - 1, p2[n + 1 - i] - 1) * 2 + 1最大

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k, 0, sizeof(k))
using namespace std;

int n;
int k[10005], s[10005], p[10005];

//二分搜
int bs(int v, int l, int r){
    int m;
    while(l<r){
        m = (l + r) / 2;
        if(v>s[m])
            l = m + 1;
        else{
            r = m;
        }
    }
    return r;
}
//LIS (nlogn解法)
void lis(void){
    int current = 1;
    for (int i = 1; i <= n; i++){
        if(k[i]>s[current-1]){
            s[current] = k[i];
            p[i] = current;
            current++;
        }
        else{
            int index = bs(k[i], 1, current);
            s[index] = k[i];
            p[i] = index;
        }
    }
}

int32_t main(void){
    while (cin >> n){
        int p1[10005], p2[10005];
        clr(k); clr(s); clr(p);
        //輸入
        for (int i = 1; i <= n; i++){
            cin >> k[i];
        }
        lis();      //第一次LIS(正向)
        for (int i = 1; i <= n; i++){
            p1[i] = p[i];
        }
        clr(s); clr(p);
        //將k反轉
        int temp[10005];
        for (int i = 1; i <= n; i++){
            temp[i] = k[i];
        }
        for (int i = 1; i <= n; i++){
            k[i] = temp[n + 1 - i];
        }
        lis();      //第二次LIS(反向，變成LDS)
        for (int i = 1; i <= n; i++){
            p2[i] = p[i];
        }
        //答案為： min(p1[i] - 1, p2[n + 1 - i] - 1) * 2 + 1;
        //p2要轉回來
        int ans = 0;
        for (int i = 1; i <= n; i++){
            int t = min(p1[i] - 1, p2[n + 1 - i] - 1) * 2 + 1;
            ans = max(ans, t);
        }
        cout << ans << '\n';
    }
}
```