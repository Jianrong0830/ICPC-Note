---
tags: Algorithm
---
# Kosaraju
```c=
#include <iostream>
#include <stack>
using namespace std;

stack<int> topo;
int scc[9];
int visited[9];
int map[9][9] =
{   {0,0,0,0,0,0,0,0,0},
    {0,0,0,1,0,0,0,0,0},
	{0,1,0,0,1,0,0,0,0},
	{0,0,1,0,1,1,0,0,0},
	{0,0,0,0,0,0,1,0,0},
	{0,0,0,0,0,0,1,1,0},
	{0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,1},
	{0,0,0,0,0,0,1,1,0}   };

void Topo(int id){
    for (int i = 1; i <= 8; i++){
        if (map[id][i] == 1 && visited[i] == 0){
            visited[i] = 1;
            Topo(i);
        }
    }
    topo.push(id);
}

void Kosaraju(int id, int scc_id){
    scc[id] = scc_id;
    for (int i = 1; i <= 8; i++){
        if (map[i][id] == 1 && visited[i] == 0){
            visited[i] = 1;
            Kosaraju(i, scc_id);
        }
    }
}

int main(void){
    for (int i = 1; i <= 8; i++){
        if(visited[i] == 0){
            visited[i] = 1;
            Topo(i);
        }
    }

    for (int i = 1; i <= 8; i++)
        visited[i] = 0;

    int scc_id = 1;
    while(topo.size() > 0){
        int id = topo.top();
        topo.pop();
        if(visited[id] == 0){
            Kosaraju(id, scc_id);
            scc_id++;
        }
    }
    for (int i = 1; i <= 8; i++){
        cout << char(64 + scc[i]) << ':' << scc[i] << ' ';
    }
}
```
```