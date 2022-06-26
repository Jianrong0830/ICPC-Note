---
tags: UVA
---
# UVA 10342 - Always Late ([Link](https://onlinejudge.org/external/103/10342.pdf))
#Dijkstra #次短路徑

## 想法
執行Dijkstra時，保存起點到每一點的 "最短距離" 和 "次短距離"

## Code
```c=
#include<bits/stdc++.h>
using namespace std;  
  
const int MAXN=105;  
int d[MAXN][MAXN][2];  
int G[MAXN][MAXN];  
int n,m;  

struct node{  
    int u,cost;  
    bool operator <(const node& a)const{  
        return cost>a.cost;  
    }  
};  

void dij(int s)  
{  
    priority_queue<node>Q;  
    for(int i=0;i<n;i++)  
        if(G[s][i])  
            Q.push((node){i,G[s][i]});  
    node t,tt;  
    while(!Q.empty())  
    {  
        t=Q.top();  
        Q.pop();  
        int u=t.u;  
        int cost=t.cost;  
        if(d[s][u][0]==0)  
        {  
            d[s][u][0]=cost;  
        }  
        else{  
            if(d[s][u][0]>cost)  
            {  
                d[s][u][1]=d[s][u][0];  
                d[s][u][0]=cost;  
            }  
            else if(d[s][u][0]==cost)continue;  
            else if(d[s][u][1]==0||d[s][u][1]>cost)  
            {  
                d[s][u][1]=cost;  
            }  
            else continue;  
        }  
        for(int i=0;i<n;i++)  
        {  
            if(G[u][i])  
                Q.push((node){i,G[u][i]+cost});  
        }  
    }  
}  
  
int main()  
{  
    int T=0;  
    while(~scanf("%d%d",&n,&m))  
    {  
        int a,b,c;  
        memset(G,0,sizeof(G));  //初始化
        //輸入測資
        for(int i = 1; i <= m; i++)  
        {  
            scanf("%d%d%d",&a,&b,&c);  
            G[a][b]=c;  
            G[b][a]=c;  
        }  
        printf("Set #%d\n",++T);  
        memset(d,0,sizeof(d));  
        //執行算法
        for(int i = 0; i < n; i++)  
            dij(i);  
        int cas;  
        scanf("%d",&cas); 
        //輸出答案
        for(int i = 1; i <= cas; i++)  
        {  
            scanf("%d%d",&a,&b);  
            if(a != b){  
                if(d[a][b][1])  
                    printf("%d\n",d[a][b][1]);  
                else printf("?\n");  
                printf("%d\n",d[a][b][0]);  
            }  
            else {  
                if(d[a][b][0])  
                    printf("%d\n",d[a][b][0]);  
                else printf("?\n");  
            }  
        }  
    }  
    return 0;  
}  
```