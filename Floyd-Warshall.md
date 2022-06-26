---
tags: Algorithm
---
# Floyd-Warshall

## 用途
針對有、無權重的有向圖做出全點全源最短路徑

## 時間複雜度
O(n3) 

## 核心概念
DP、3個迴圈

## Code
```c=
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define MAXN 105
using namespace std;

int n,m;
int dist[MAXN][MAXN];

void init(){
    for(int i=1;i<=n; i++){
        for(int j=1;j<=n;j++){
            dist[i][j]=INF;
        }
        dist[i][i]=0;
    }
}

void floyd(){
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(dist[i][k]+dist[k][j]<dist[i][j]){
                    dist[i][j]=dist[i][k]+dist[k][j];
                }
            }
        }
    }
}

int main(void){
    cin>>n>>m;
    init();
    int a,b,c;
    while(m--){
        cin>>a>>b>>c;
        dist[a][b]=c;
    }
    floyd();
    cout<<"最短路徑： "<<dist[1][n]<<'\n';
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++) cout<<dist[i][j]<<' ';
        cout<<'\n';
    }
}
```


## 範例測資
input:
4 8
1 2 2
1 3 6
1 4 4
2 3 3
3 1 7
3 4 1
4 1 5
4 3 12

output:
4