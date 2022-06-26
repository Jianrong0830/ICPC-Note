---
tags: UVA
---
# UVA 11753 - Creating Palindrome
```c=
#include <iostream>
#define maxans 21
using namespace std;

int t;
int n, k;
int num[10000];
int ans = maxans;

void init(){
    for (int i = 0; i < 10000; i++){
        num[i] = 0;
        ans = maxans;
    }
}

void dfs(int l, int r, int d){
    if(l>=r){
        ans = min(ans, d);
        return;
    }
    if(d>k){
        ans = min(k + 1, ans);
        return;
    }
    if(num[l]==num[r]){
        dfs(l + 1, r - 1, d);
    }
    else{
        dfs(l + 1, r, d + 1);
        dfs(l, r - 1, d + 1);
    }
}

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        init();
        cin >> n >> k;
        for (int i = 0; i < n; i++){
            cin >> num[i];
        }
        dfs(0, n-1, 0);
        cout << "Case :" << i << ": ";
        if(ans>k){
            cout << "Too difficult";
        }
        else if(ans==0){
            cout << "Too easy";
        }
        else{
            cout << ans;
        }
        cout << '\n';
    }
}
```