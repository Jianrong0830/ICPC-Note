---
tags: UVA
---

# UVA 10931 - Parity ([Link](https://vjudge.net/problem/UVA-10931))
#二進位

## 想法
十進位轉二進位

## Code
```c=
#include <iostream>
#include <string.h>
using namespace std;

int n;

int main(void){
    while(cin>>n && n){
        string s = "";
        int ans = 0;
        while(n>0){
            if(n&1){
                s = '1' + s;
                ans++;
            }
            else
                s = '0' + s;
            n >>= 1;
        }
        cout << "The parity of " << s << " is " << ans << " (mod 2).\n";
    }
}
```