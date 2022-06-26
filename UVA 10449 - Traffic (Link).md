---
tags: UVA
---
# UVA 10449 - Traffic ([Link](https://onlinejudge.org/external/104/10449.pdf))
#bellman_ford

## 想法
因為有負權邊，所以用bellman_ford
?的情況：
1.有負權迴路
2.答案<3
3.答案=INF

## Code
```c=
#include <iostream>
#include <vector>
#include <string.h>
#define inf 0x3f3f3f3f
#define MAXN 205
using namespace std;

int n,r;
int value[MAXN];
int u[MAXN],v[MAXN],w[MAXN];
int q;
int dist[MAXN];
vector<int> s;

void init(){
    memset(u,0,sizeof(u));
    memset(v,0,sizeof(v));
    memset(w,0,sizeof(w));
    memset(dist,inf,sizeof(dist));
}

void bellman(int root){
    dist[root]=0;
    for(int i=1;i<n;i++){
        for(int j=1;j<=r;j++){
            if(dist[u[j]]+w[j]<dist[v[j]]){
                dist[v[j]]=dist[u[j]]+w[j];
            }
        }
    }
    //有負權迴路就丟進vector
    for(int i=1;i<=r;i++){
        if(dist[u[i]]+w[i]<dist[v[i]]){
            dist[v[i]]=dist[u[i]]+w[i];
            s.push_back(v[i]);
        }
    }
}

int main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    int T = 1;
    while(cin>>n && n){
        init(); //初始化
        //輸入測資
        for(int i=1;i<=n;i++) cin>>value[i];
        cin>>r;
        for(int i=1;i<=r;i++){
            cin>>u[i]>>v[i];
            int temp=value[v[i]]-value[u[i]];
            w[i]=temp*temp*temp;
        }
        //執行算法
        bellman(1);
        //輸出答案
        cout << "Set #"<<T << '\n';
        cin>>q;
        for(int i=1;i<=q;i++){
            int e;
            cin >> e;
            if(s.size() || dist[e]<3 || dist[e]==inf)
                cout << "?\n";
            else
                cout << dist[e] << '\n';
        }
        T++;
    }
}
```