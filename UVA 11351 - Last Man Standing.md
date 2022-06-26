---
tags: UVA
---
# UVA 11351 - Last Man Standing
遞迴版
```c=
#include <iostream>
using namespace std;

int t;

int Josephus(int n,int k){
    if(n==1)
        return 0;
    return (Josephus(n - 1, k) + k) % n;
}

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        int n, k;
        cin >> n >> k;
        cout << "Case " << i << ": " << Josephus(n, k) + 1 << '\n';
    }
}
```
DP版
```c=
#include <iostream>
using namespace std;

int t;

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        int n, k;
        cin >> n >> k;
        int dp[n + 1];
        dp[1] = 0;
        for (int j = 2; j <= n; j++){
            dp[j] = (dp[j - 1] + k) % j;
        }
        cout << "Case " << i << ": " << dp[n] + 1 << '\n';
    }
}
```