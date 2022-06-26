---
tags: UVA
---
# UVA 12893 - Count It
#腦筋急轉彎、二進位
## 思路
答案為： 將測資轉為二進位後，1的個數
## Code
```c=
#include <iostream>
#define int long long
using namespace std;

int t;

int32_t main(void){
    cin >> t;
    for (int i = 1;i<=t; i++){
        int n;
        int ans = 0;
        cin >> n;
        while(n){               //若n大於0就持續執行
            ans+=n%2;           //若餘數為1就+1，否則+0
            n >>= 1;            //n=int((n-n%2)/2)
        }
        cout << ans << '\n';
    }
}
```