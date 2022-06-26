---
tags: UVA
---

# UVA 11384 - Help is needed for Dexter ([Link](https://onlinejudge.org/external/113/11384.pdf))
#數學

## 想法
找到中點，從中點開始到最後都減掉中點，中點會歸零，下一個會從1開始循環
此操作會分割成兩段數列，每一段再做重複的操作，直到整個數列變成0、1相間隔
最後再一次消掉所有1
答案歸納出來為：int(log(2,n))+1

## Code
```c=
#include <iostream>
using namespace std;

int n;

int main(void){
    while (cin >> n){
        int ans = 1;
        while(n>=2){
            n /= 2;
            ans++;
        }
        cout << ans << '\n';
    }
}
```