---
tags: UVA
---
# UVA 12190	- Electric Bill
#二分搜

## 想法
用二分搜尋找鄰居的用電量(c2)，並算出b2,c1,c2，得到一個差值b2-b1
若此差值比題目的B大，代表此b2太大，於是搜尋左邊，反之搜尋右邊

## Code
```c=
#include <iostream>
#include <algorithm>
#define int long long
#define INF 2147483647
using namespace std;

int a, b, ct;

int c_culculating(int32_t b){       //consumption to bill
    int ans = 0;
    ans += min(100-0, b / 2);
    b -= min(100*2, b);
    ans += min(10000-100, b / 3);
    b -= min((10000-100)*3, b);
    ans += min(1000000-10000, b / 5);
    b -= min((1000000-10000)*5, b);
    ans += b / 7;
    return ans;
}

int b_culculating(int32_t c){       //bill to consumption
    int ans = 0;
    ans += max((c - 1000000),0) * 7;
    c -= max((c - 1000000), 0);
    ans += max((c - 10000),0) * 5;
    c -= max((c - 10000), 0);
    ans += max((c - 100),0) * 3;
    c -= max((c - 100), 0);
    ans += c * 2;
    return ans;
}

//用二分搜尋找鄰居的用電量(c2)，並算出b2,c1,c2，得到一個差值b2-b1
//若此差值比題目的B大，代表此b2太大，於是搜尋左邊，反之搜尋右邊

int bsearch(int l, int r){
    int mid = (l + r) / 2;
    int c2 = mid, c1 = ct - mid;
    int b2 = b_culculating(c2), b1 = b_culculating(c1);
    if(b2-b1==b)
        return b1;
    if(b2-b1>b)
        return bsearch(l, mid);
    return bsearch(mid + 1, r);
}

int32_t main(void){
    while(cin>>a>>b && (a || b)){
        ct = c_culculating(a);
        cout << bsearch(0, ct) << '\n';
    }   
}

```