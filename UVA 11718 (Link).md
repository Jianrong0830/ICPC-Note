---
tags: UVA
---
# UVA 11718 ([Link](https://onlinejudge.org/external/117/11718.pdf))
#Math

## 想法
程式碼是呈現一種組合，類似於二進為遞增，不過0變成1，1變成2
用快速次方計算答案
每次算完還要再mod以避免overflow

## Code
```c=
#include <iostream>
#define MAXN 1005
using namespace std;

int num[MAXN];
int t, n, k, m;

//快速次方
int pow(int x, int y){
    if(y == 0)
        return 1;
    int ans = pow(x, y / 2) % m;
    if(y % 2 == 1)
        ans = (ans * ans % m) * x % m;
    else
        ans = ans * ans % m;
    return ans;
}

int main(){
    cin >> t;
    for (int k = 1; k <= t; k++){
        //輸入測資
        cin >> n >> k >> m;
        for(int i = 0; i < n; i++)
            cin >> num[i];
        //執行算法
        int sum = num[0];
        for(int i = 1; i < n; i++)
            sum = (sum + num[i]) % m;
        //輸出答案
        int ans = (sum * (pow(n, k - 1) % m) * (k % m)) % m;
        cout << "Case " << k << ": " << ans << "\n";
    }
}
```