---
tags: Algorithm
---
# BFS
```cpp=
#include <iostream>
#include <queue>

using namespace std;

queue<int> q;
int visited[7];
int map[7][7] =
{   {0,0,0,0,0,0,0},
	{0,0,1,1,0,0,0},
	{0,1,0,0,1,0,1},
	{0,1,0,0,0,0,1},
	{0,0,1,0,0,0,0},
	{0,0,0,0,0,0,1},
	{0,0,1,1,0,1,0}   };

int main(void){
    visited[1] = 1;
    q.push(1);
    while(q.size() > 0){
        int id = q.front();
        cout << id << ' ';
        for (int i = 1; i <= 6; i++){
            if(map[id][i] == 1 && visited[i] == 0){
                visited[i] = 1;
                q.push(i);
            }
        }
        q.pop();
    }
}
```