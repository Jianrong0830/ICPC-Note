---
tags: Algorithm
---
# LIS

## 用途
最大遞增子序列

## 時間複雜度
O(nlogn)

## 核心概念
二分搜

## Code 
Array版本 (有輸出LIS陣列)
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

int a[MAXN];
int t[MAXN],d[MAXN];
int n;

int bs(int l, int r, int v){
    while(r>l){
        int m=(l+r)/2;
        if(a[v]>a[t[m]]) l=m+1;
        else r=m;
    }
    return l;
}

int lis(){
    int len=1;
    t[1]=d[1]=1;
    for(int i=2;i<=n;i++){
        if(a[i]>a[t[len]]){
            t[++len]=i;
            d[i]=len;
        }
        else{
            int temp=bs(1,len,i);
            t[temp]=i;
            d[i]=temp;
        }
    }
    return len;
}

int main(void){
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    int ans=lis();
    cout<<ans<<'\n';
    stack<int> s;
    for(int i=n;i>=1;i--){
        if(ans==0) break;
        if(d[i]==ans){
            s.push(a[i]);
            ans--;
        }
    }
    while(!s.empty()){
        cout<<s.top()<<' ';
        s.pop();
    }
}
```

Deque版本 (只輸出LIS長度)
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

int a[MAXN];
int t[MAXN],d[MAXN];
int n;

int lis(){
    deque<int> q;
    q.push_back(a[1]);
    for(int i=2;i<=n;i++){
        if(a[i]>q.back()) q.push_back(a[i]);
        else{
            int temp=upper_bound(q.begin(),q.end(),a[i])-q.begin();
            q[temp]=a[i];
        }
    }
    return q.size();
}

int main(void){
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    cout<<lis();
}
```

## 範例測資
input:
7
2 1 4 3 6 7 5

output:
4
(1 3 6 7)