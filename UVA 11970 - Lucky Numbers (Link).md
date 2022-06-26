---
tags: UVA
---

# UVA 11970 - Lucky Numbers ([Link](https://onlinejudge.org/external/119/11970.pdf))
#數學

## 想法
不能全部都跑一遍，會TLE
要過濾(根號為整數的再跑)

## Code
```c=
#include <iostream>
#include <math.h>
#define MAXN 200005
#define INF 0x3f3f3f
using namespace std;

int t, n;

int32_t main(void){
    cin >> t;
    for (int k = 1; k <= t; k++){
        cout << "Case " << k << ": ";
        cin >> n;
        for (int i = sqrt(n)-1; i >= 1; i--){
            int x = n - i * i;
            if(x%i==0)
                cout << x << ' ';
        }
        cout << '\n';
    }
}
```