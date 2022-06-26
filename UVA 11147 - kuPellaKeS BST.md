---
tags: UVA
---
# UVA 11147 - kuPellaKeS BST
```c=
#include <iostream>
#include <algorithm>
#define int long long
#define INF 2147483647
using namespace std;

int t, n, num[4020];
int bst[4020];
int flag[4020];
int pre[4020];

void init(){
    for (int i = 0; i < 4020; i++){
        bst[i] = 0, flag[i] = 0, num[i] = 0, pre[i] = 0;
    }
}

void build_pre(){
    for (int i = 1; i<=n; i++){
        pre[i] = pre[i - 1] + num[i];
    }
}

void dfs(int id, int l, int r){
    if(l>r){
        return;
    }
    int min_difference = INF;
    int root;
    for (int i = r; i >= l; i--){
        int l_sum = pre[i - 1] - pre[l - 1];
        int r_sum = pre[r] - pre[i];
        if(abs(r_sum-l_sum)<min_difference && (i==r || num[i+1]>num[i])){
            min_difference = abs(r_sum - l_sum);
            root = i;
        }
    }
    bst[id] = num[root];
    flag[id] = 1;
    dfs(id * 2, l, root - 1);
    dfs(id * 2 + 1, root + 1, r);
}

void print(int id){
    if(flag[id]==0){
        return;
    }
    cout << bst[id];
    if(flag[id*2] || flag[id*2+1]){
        cout << '(';
    }
    print(id * 2);
    print(id * 2 + 1);
    if(id%2==0 && flag[id+1]==1){
        cout << ',';
    }
    else if(flag[id*2] || flag[id*2+1]){
        cout << ')';
    }
}

int32_t main(void){
    cin >> t;
    for(int i=1; i<=t; i++){
        cin >> n;
        init();
        for (int i = 1; i <= n; i++){
            cin >> num[i];
        }
        sort(num + 1, num + n + 1);
        build_pre();
        dfs(1, 1, n);
        cout << "Case #" << i << ": ";
        print(1);
        cout << '\n';
    }
}
```