---
tags: UVA
---
# UVA 13142 - Destroy the Moon to Save the Earth
#水題
## 想法
距離/時間=速率

## Code
```c=
#include <iostream>
#include <math.h>
#define int long long
using namespace std;

int n;
int t, s, d;

int32_t main(void){
    cin >> n;
    while(n--){
        cin>>t>>s>>d;
        //要注意單位轉換
        int time = t * 24 * 60 * 60;
        int distance = d * 1000000;
        double ans = distance / time;
        //若為正，須加速，減重
        if(ans>0){
            cout << "Remove " << floor(ans) << " tons" << '\n';
        }
        //若為負，需減速，增重
        else{
            cout<<"Add "<<floor(abs(ans)) << " tons" << '\n';
        }
    }
}
```