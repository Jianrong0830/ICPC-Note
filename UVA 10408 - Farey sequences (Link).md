---
tags: UVA
---

# UVA 10408 - Farey sequences ([Link](https://onlinejudge.org/external/104/10408.pdf))
#數學 #Farey數列

## 想法
t=gcd(a0+a2, b0+b2)
t越大下一位越小 → 讓t最大

當前maxt算法(以第2位為例)
![](https://i.imgur.com/i1cJ1lH.jpg)
a[2] = 當前maxt * a[1] - a[0], b[2] = 當前maxt * b[1] - b[0];

最後公式：
a[i] = 當前maxt * a[i - 1] - a[i - 2], b[i] = 當前maxt * b[i - 1] - b[i - 2];



## Code
```c=
#include <iostream>
#include <string.h>
#define MAXN 400000   //0.3039635n^2
#define INF 0x3f3f3f
#define int long long
using namespace std;

int n, k;
int a[MAXN], b[MAXN];

//算出整個Farey數列(到第k位)
void farey(int n, int k){
    a[0] = 0, b[0] = 1;
    a[1] = 1, b[1] = n;
    for (int i = 2; i <= k; i++){
        int t = (n + b[i - 2]) / b[i - 1];
        a[i] = t * a[i - 1] - a[i - 2], b[i] = t * b[i - 1] - b[i - 2];
    }
}

int32_t main(void){
    while(cin>>n>>k){
        memset(a, 0, MAXN);
        memset(b, 0, MAXN);
        farey(n, k);
        cout << a[k] << '/' << b[k] << '\n';
    }
}
```