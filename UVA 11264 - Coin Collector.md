---
tags: UVA
---
# UVA 11264 - Coin Collector
## 想法
將硬幣由小排到大(題目已經做到了)
便利一遍，持續紀錄選到的硬幣總額
選取條件：此硬幣大於當前總額，且新總額也會小於下一個硬幣(不會被下一個硬幣搶走優先權)

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
using namespace std;

int t, n;
int c[1001];

int32_t main(void){
    cin >> t;
    while(t--){
        memset(c, 0, sizeof(c));
        cin >> n;
        for (int i = 1;i<=n; i++){
            cin >> c[i];
        }
        int ans = 2, sum = c[1];        //先把頭尾算進去，初始的sum為c[1]
        for (int i = 2; i < n; i++){
            //若遍歷到的硬幣大於當前總額，且選上後的新總額小於下一個硬幣(不會被下一個硬幣搶走優先權)，則選取
            if(sum<c[i] && sum+c[i]<c[i+1]){
                sum += c[i];
                ans++;
            }
        }
        cout << ans << '\n';
    }
}
```
        