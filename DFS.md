---
tags: Algorithm
---
# DFS
```c=
#include <iostream>
using namespace std;

int visited[7];
int map[7][7] =
{  {0,0,0,0,0,0,0},
	{0,0,1,1,0,0,0},
	{0,1,0,0,1,0,1},
	{0,1,0,0,0,0,1},
	{0,0,1,0,0,0,0},
	{0,0,0,0,0,0,1},
	{0,0,1,1,0,1,0} };

void dfs(int id){
    cout << id << ' ';
    for (int i = 1; i <= 6; i++){
        if(map[id][i] == 1 && visited[i] == 0){
            visited[i] = 1;
            dfs(i);
        }
    }
}
int main(void){
    visited[1] = 1;
    dfs(1);
}
```