---
tags: UVA
---
# UVA 10774 - Repeated Josephus
遞迴
```c=
#include <iostream>
using namespace std;

int t;

int Josephus(int n, int k = 2){
    if(n==1)
        return 0;
    return (Josephus(n - 1, k) + k) % n;
}

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        int n;
        cin >> n;
        int survivor = 0;
        int time = 0;
        while(true){
            survivor = Josephus(n) + 1;
            time++;
            if(survivor==n){
                break;
            }
            n = survivor;
        }
        cout << "Case " << i << ": " << time - 1 << ' ' << survivor << '\n';
    }
}
```
DP
```c=
#include <iostream>
using namespace std;

int t;
int dp[30001];

int main(void){
    cin >> t;
    dp[1] = 0;
    for (int i = 2;i<=30000; i++){
        dp[i] = (dp[i - 1] + 2) % i;
    }
    for (int i = 1; i <= t; i++){
        int n;
        cin >> n;
        int survivor = 0;
        int time = 0;
        while(true){
            survivor = dp[n] + 1;
            time++;
            if(survivor == n){
                break;
            }
            n = survivor;
        }
        cout << "Case " << i << ": " << time - 1 << ' ' << survivor << '\n';
    }
}
```