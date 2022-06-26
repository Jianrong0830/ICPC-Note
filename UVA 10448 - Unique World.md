---
tags: UVA
---
# UVA 10448 - Unique World
#DFS #DP
## 想法
超麻煩，DFS+DP
用DFS先走訪，看能不能走到，同時required_cost扣掉走過的路的花費
No的可能：
1.到不了 2.required_cost太小 3.剩餘的cost為奇數(不可能用重複走的方式用掉) 4.還有多餘的cost但沒有多的路
若不是NO則用DP運算答案
## Code
```c=
#include <iostream>
#include <string.h>
#include <vector>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
#define INF 2147483647
using namespace std;

int N;
int n, k;
int q;
int map[55][55], visited[55];       //鄰接陣列
int from, to, rc = 0;  //required_cost
vector<int> travel_cost;
int dp[55][100005];

int dfs(int id){
    if(id == to){       //已到達
        return 1;
    }
    if(visited[id])     //已走訪過，無法繼續走訪
        return 0;
    visited[id] = 1;
    for (int i = 1; i <= n; i++){       //搜尋可能的節點
        if(map[id][i] > 0 && dfs(i)){
            rc -= map[id][i];
            travel_cost.push_back(map[id][i]);
            return 1;
        }
    }
    return 0;
}

int32_t main(void){
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin >> N;
    while(N--){
        clr(map);
        cin >> n >> k;
        while(k--){
            int id1, id2, c;
            cin >> id1 >> id2 >> c;
            //雙向的圖，都要記錄
            map[id1][id2] = c;
            map[id2][id1] = c;
        }

        cin >> q;
        while(q--){
            clr(visited);
            //二維用memset有bug
            for (int i = 0; i < 55;i++){
                for (int j = 0; j < 10005; j++)
                    dp[i][j] = INF;
            }
            travel_cost.clear();
            cin >> from >> to >> rc;
            int accessable = dfs(from);
            //No的可能：1.到不了 2.required_cost太小 3.剩餘的cost為奇數(不可能用重複走的方式用掉) 4.還有多餘的cost但沒有多的路
            if(!accessable || rc<0 || rc%2==1 || (rc>0 && travel_cost.size()==0)){
                cout << "No" << '\n';
                continue;
            }
            //每條路走一次剛好用完cost
            if(rc==0){
                cout << "Yes" << ' ' << travel_cost.size() << '\n';
                continue;
            }
            rc /= 2;
            dp[0][0] = 0;
            //處理DP
            for (int i = 1; i < travel_cost.size(); i++){
                for (int j = 0; j <= rc; j++){
                    if(j>=travel_cost[i])
                        dp[i][j] = min(dp[i][j], dp[i][j - travel_cost[i]] + 1);
                    if(i>0)
                        dp[i][j] = min(dp[i][j], dp[i - 1][j]);
                }
            }
            //若DP沒有答案族輸出NO
            if(dp[travel_cost.size()-1][rc]==INF){
                cout << "No" << '\n';
            }
            else{
                cout << "Yes" << ' ' << travel_cost.size() + dp[travel_cost.size() - 1][rc] * 2 << '\n';
            }
        }
    }
}
```