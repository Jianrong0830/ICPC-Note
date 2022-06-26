---
tags: UVA
---
# UVA 12834 - Extreme Terror (WA)
水題
```c=
#include <iostream>
#include <algorithm>
#define int long long
using namespace std;

int t;
int n, k;
int p[1000001];
int x[1000001], y[1000001];

void init(){
    for (int i = 0; i <= 1000000; i++){
        p[i] = 0, x[i] = 0, y[i] = 0;
    }
}

int32_t main(void){
    cin >> t;
    int sum = 0;
    for (int i = 1; i <= t; i++){
        init();
        cin >> n >> k;
        for (int j = 1; j <= n; j++){
            cin >> x[j];
        }
        for (int j = 1; j <= n; j++){
            cin >> y[j];
        }
        for (int j = 1; j <= n; j++){
            p[j] = y[j] - x[j];
            sum += p[j];
        }
        sort(p + 1, p + n + 1);
        for (int j = 1; j <= k; j++){
            if(p[j]<0){
                sum -= p[j];
            }
        }
        cout << "Case " << i << ": ";
        if(sum<=0){
            cout << "No Profit" << '\n';
        }
        else{
            cout << sum << '\n';
        }
    }
}
```