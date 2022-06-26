---
tags: UVA
---

# UVA 12416 - Excessive Space Remover ([Link](https://onlinejudge.org/external/124/p12416.pdf))
#數學

## 想法
找出最長的連續空格數，不斷進行+1再/2的動作直到變成1為止，操作次數即為答案

## Code
```c=
#include <iostream>
#include <string.h>
using namespace std;

string s;

int main(void){
    while(getline(cin, s)){
        int max_blank = 0;
        int l = s.length();
        int temp = 0;
        for (int i = 0; i < l; i++){
            if(s[i]==' ')
                temp++;
            else{
                max_blank = max(max_blank, temp);
                temp = 0;
            }
        }
        int ans = 0;
        while(max_blank>1){
            max_blank = (max_blank + 1) / 2;
            ans++;
        }
        cout << ans << '\n';
    }
}
```