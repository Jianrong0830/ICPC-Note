---
tags: UVA
---
# UVA 10346 - Peter's Smokes
```c=
#include <iostream>
using namespace std;

int n, k;

int main(void){
    while(cin>>n>>k){
        int ans = 0;
        for (int i = 1; i <= n;i++){
            ans++;
            if(i%k==0){
                n++;
            }
        }
        cout << ans << endl;
    }
    
}
```