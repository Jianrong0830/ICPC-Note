---
tags: UVA
---
# UVA 11742 - Social Constraints
排列組合 暴搜
```c=
#include <iostream>
#include <string>
using namespace std;

int n, m;
string seats;
int visited[8];
char A[20], B[20];          //用A、B、C陣列紀錄條件
int C[20];
int ans = 0;

void init(){            //歸零
    ans = 0;
    for (int i = 0; i < 20; i++){
        A[i] = 0, B[i] = 0, C[i] = 0;
        if(i<8){
            visited[i] = 0;
        }
    }
}

void dfs(string arrange, int cnt){          //排列組合
    if(cnt==n){
        bool possible = true;               //紀錄此組合可不可行
        for (int i = 0; i < m; i++){        //判斷此組合可不可行
            int d = arrange.find(A[i]) - arrange.find(B[i]);
            d = abs(d);         //距離
            if(C[i]>0){
                if(d>C[i]){
                    possible = false;
                }
            }
            else{
                if(d<-C[i]){
                    possible = false;
                }
            }
        }
        if(possible){
            ans++;
        }
        return;
    }
    for (int i = 0; i < n; i++){
        if(visited[i]==0){
            string temp = arrange + char(int('0' + i));         //若直接用arrange會出問題
            visited[i] = 1;
            dfs(temp, cnt + 1);
            visited[i] = 0;
        }
    }
}

int main(void){
    while(cin>>n>>m && (n||m)){
        init();
        for (int i = 0; i < m; i++){
            cin >> A[i] >> B[i] >> C[i];
        }
        dfs("", 0);
        cout << ans << endl;
    }
}
```