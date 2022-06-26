---
tags: UVA
---
# UVA 10954 - Add All
#priority queue

## 想法
利用priority queue，每次取前兩小的相加，加到ans
每次相加的結果再丟回priority queue
priority queue升冪寫法： priority_queue<int,vector<int>,greater<int>> q;

## Code
```c=
#include <iostream>
#include <queue>
#include <string.h>
#define int long long
using namespace std;

int n;

int32_t main(void){
    while(cin>>n && n){
        //升冪寫法
        priority_queue<int,vector<int>,greater<int>> q;
        for (int i = 0; i < n; i++){
            int temp;
            cin >> temp;
            q.push(temp);
        }
        int ans = 0;
        while(q.size()>1){
            int temp = 0;       //temp紀錄每次前兩個相加的結果
            temp += q.top(), q.pop();
            temp += q.top(), q.pop();
            ans += temp;        //把temp加到ans
            q.push(temp);       //還要把temp再丟回priority queue
        }
        cout << ans << '\n';
    }
}
```