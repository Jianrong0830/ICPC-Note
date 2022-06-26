---
tags: UVA
---
# UVA 13054 - Hippo Circus
greedy
```c=
#include <iostream>
#include <algorithm>
#define int long long
using namespace std;

int c;
int n, h, ta, td;
int x[100001];
int choosed[100001];

void init(){
    for (int i = 0;i<=100001; i++){
        x[i] = 0, choosed[i] = 0;
    }
}

int32_t main(void){
    cin >> c;
    for (int i = 1; i <= c; i++){
        init();
        cin >> n >> h >> ta >> td;
        for (int j = 1; j <= n; j++){
            cin >> x[j];
        }
        int time = ta * n;
        if(ta*2 > td){
            sort(x + 1, x + n + 1);
            int l = 1;
            for (int r = n; r >= 1; r--){
                if(r<=l){
                    break;
                }
                if(x[l] + x[r] < h){
                    l++;
                    time = time - ta * 2 + td;
                }
            }
        }
        cout << "Case " << i << ": " << time << '\n';
    }
}
```