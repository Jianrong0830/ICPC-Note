---
tags: UVA
---
# UVA 12249 - Overlapping Scenes
排列組合 暴搜
```c=
#include <iostream>
#include <string>
#define INF 2147483647

using namespace std;

int t, n;
string scenes[6];
int visited[6];
int ans = INF;
//string ans_str;       Debug

void init(){        //歸零
    ans = INF;
    for (int i = 0; i < 6; i++){
        scenes[i] = '\0';
        visited[i] = 0;
    }
}

void dfs(string merged_scene, int scenes_cnt){      //用DFS作排列組合
    if (scenes_cnt == n){                           //n個scenes都排好就跳出遞迴
        if (merged_scene.size() < ans){             //找出最小的長度
            ans = merged_scene.size();
            //ans_str = merged_scene;               //Debug
        }
        return;
    }
    for (int i = 0; i < n; i++){
        if(visited[i]==0){
            int max_len = min(merged_scene.size(), scenes[i].size());           //最大的可能重複長度為較短字串的長度
            string temp;                                                        //直接用merged_scene會出問題
            for (int l = max_len; l >= 0; l--){                                 //從最大的可能重複長度開始找重複子字串，第一個找到的一定可將兩字串壓縮到最小長度
                if(merged_scene.substr(merged_scene.size()-l)==scenes[i].substr(0,l)){
                    temp = merged_scene + scenes[i].substr(l);
                    break;
                }
            }
            visited[i] = 1;
            dfs(temp, scenes_cnt + 1);
            visited[i] = 0;                                                     //回到原本遞迴就將剛剛走的節點設為沒走過(因為之後還會走到)
        }
    }
}

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        init();
        cin >> n;
        for (int j = 0; j < n; j++){
            cin >> scenes[j];
        }
        dfs("", 0);
        cout << "Case " << i << ": " << ans << endl;
        //cout << "Merged Scene: " << ans_str << endl;                          //Debug
    }
}
```