---
tags: UVA
---
# UVA 11792 - Krochanska is Here!
#BFS

## 想法
每隔重點車站都用BFS走訪，找到到其他重點車站的最短距離，並算出平均值(用double)
找到能使此平均值最小的重點車站即為答案

## Code
```c=
#include <iostream>
#include <vector>
#include <queue>
#include <string.h>
#define int long long
#define MAXN 10020
#define INF 0x3f3f3f
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int t;
int n, s;
int cnt[MAXN];
vector<vector<int>> edge;
int dist[MAXN], visited[MAXN];
double min_mean_dist = INF;
int ans = 0;
vector<int> important_station;

void BFS(int root){
    deque<int> q;
    q.push_back(root);
    visited[root] = 1;
    dist[root] = 0;
    while(!q.empty()){
        int id = q.front();
        q.pop_front();
        for(auto& i:edge[id]){
            if(!visited[i]){
                q.push_back(i);
                visited[i] = 1;
                dist[i] = dist[id] + 1;
            }
        }
    }
}

int32_t main(void){
    //輸入輸出優化
    ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

    cin>>t;
    while(t--){
        //初始化
        clr(cnt); clr(dist); clr(visited);
        edge.assign(MAXN, vector<int>());
        important_station.clear();
        min_mean_dist = INF;
        ans = 0;
        //輸入測資
        cin >> n >> s;
        while(s--){
            int c;
            cin >> c;
            cnt[c]++;
            int last = c;
            while(cin>>c && c){
                cnt[c]++;
                edge[last].push_back(c);
                edge[c].push_back(last);
                last = c;
            }
        }
        //找出重點車站
        for (int i = 1; i <= n; i++){
            if(cnt[i]>1)
                important_station.push_back(i);
        }
        //執行算法
        for(int i:important_station){
            clr(dist); clr(visited);
            BFS(i);
            double total = 0;
            for(int j:important_station)
                total += dist[j];
            double mean = total / (important_station.size() - 1);
            //cout << "DEBUG: " << i << ' ' << mean << '\n';
            if(mean<min_mean_dist){
                min_mean_dist = mean;
                ans = i;
            }
        }
        //輸出答案
        cout << "Krochanska is in: " << ans << '\n';
    }
}
```