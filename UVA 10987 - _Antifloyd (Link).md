---
tags: UVA
---
# UVA 10987 - 	Antifloyd ([Link](https://onlinejudge.org/external/109/10987.pdf))
#Floyd

## 想法
若floyd可鬆弛則代表Need better measurements.
若map[i][k]+map[k][j]==map[i][j]則代表cable(i,j)可移除
最後輸出剩下沒被移除得即為答案

## Code
```c=
#include <iostream>
#define inf 0x3f3f3f

using namespace std;

int t;
int n;
int map[105][105];
int edge[105][105];
bool no_ans;
int cnt;

void init(){
    for(int i=0;i<105;i++){
        for(int j=0;j<105;j++){
            map[i][j]=inf;
            edge[i][j]=1;
        }
        map[i][i]=0;
    }
    cnt=0;
    no_ans=0;
}

void anti_floyd(){
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(map[i][k]+map[k][j]<map[i][j]){
                    cout << "Need better measurements.\n";
                    no_ans=1;
                    return;
                }
                if(i==j || j==k || i==k) continue;
                if(map[i][k]+map[k][j]==map[i][j]){
                    edge[i][j]=0;
                }
            }
        }
    }
}

int main(void){
    cin>>t;
    for(int k=1;k<=t;k++){
        init();     //初始化
        //輸入測資
        cin>>n;
        for(int i=1;i<n;i++){
            for(int j=1;j<=i;j++){
                int temp;
                cin>>temp;
                map[i+1][j]=map[j][i+1]=temp;
            }
        }
        cout<<"Case #"<<k<<":\n";
        //執行算法
        anti_floyd();
        for(int i=1;i<=n;i++){
            for(int j=i+1;j<=n;j++){
                if(edge[i][j]) cnt++;
            }
        }
        //輸出答案
        if(!no_ans){
            cout<<cnt<<'\n';
            for(int i=1;i<=n;i++){
                for(int j=i+1;j<=n;j++){
                    if(edge[i][j]){
                        cout<<i<<' '<<j<<' '<<map[i][j]<<'\n';
                    }
                }
            }
        }
        cout<<'\n';
    }
    
}
```