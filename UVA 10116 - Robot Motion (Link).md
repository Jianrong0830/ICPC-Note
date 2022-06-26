---
tags: UVA
---
# UVA 10116 - Robot Motion ([Link](https://onlinejudge.org/external/101/10116.pdf))
#DFS
## 想法
用DFS走訪地圖，開一個cnt遞增步數，一個step二維陣列紀錄走到每個點的步數
在兩個情況時跳出並輸出答案：
1.超出範圍
2.已被走訪，有loop
沒有loop時，答案的step為cnt-1
有loop時，答案的step為第一次走到重複走訪的點的步數(step[x][y]) - 1；答案的loop為cnt - step[x][y]

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int a, b, c;
string map[11];
int step[11][11];			//紀錄每個點的步數
int visited[11][11];
int cnt, s, l;				//用cnt來遞增步數，s(step),l(loop)為答案

//用DFS走訪地圖
void dfs(int x, int y){
	//超出範圍，完成，故跳出
	if(x==0 || y==0 || x>a || y>b){
		s = cnt - 1;
		l = 0;
		return;
	}
	//已被走訪，有loop，故跳出
	if(visited[x][y]){
		s = step[x][y] - 1;
		l = cnt - step[x][y];
		return;
	}
	step[x][y] = cnt;
	cnt++;
	visited[x][y] = 1;
	char current = map[x][y];
	if(current == 'N')
		dfs(x - 1, y);
	if(current == 'S')
		dfs(x + 1, y);
	if(current == 'W')
		dfs(x, y - 1);
	if(current == 'E')
		dfs(x, y + 1);
}

int32_t main(void){
	while(cin>>a>>b>>c && a){
		for (int i = 0; i <= 10; i++)
			map[i] = " ";
		clr(step); clr(visited);
		cnt = 1, s = 0, l = 0;
		for (int i = 1; i <= a; i++){
			string temp;
			cin >> temp;
			map[i] += temp;
		}
		dfs(1, c);
		if(l==0){		//沒有loop
			cout << s << " step(s) to exit\n";
		}
		else{			//有loop
			cout << s << " step(s) before a loop of " << l << " step(s)\n";
		}
	}
}
```