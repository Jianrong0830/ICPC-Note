---
tags: UVA
---
# UVA 11231 ([Link](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=2172))
#Math

## 想法
有規律，推出來答案為 (n-7) * (m-7) / 2


## Code
```c=
#include <iostream>
#define MAXN 100005
using namespace std;

int n, m, c;

int main(){
    while(cin >> n >> m >> c && n){
        int ans = (n - 7) * (m - 7) / 2;
        cout << ans << "\n";
    }
}
```