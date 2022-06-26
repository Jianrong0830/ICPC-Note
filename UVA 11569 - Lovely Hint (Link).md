---
tags: UVA
---
# UVA 11569 - Lovely Hint ([Link](https://onlinejudge.org/external/115/11569.pdf))
#DP

## 想法
比較麻煩的DP，也可以用DFS解

## Code
```c=
#include <iostream>
#include <cstdio>
#include <string.h>
#include <vector>
#include <algorithm>
#include <cstring>
#define maxn 30
using namespace std;

struct Dp{
	int vis;
	int len;
	int achi;
	Dp(int a = 0 , int b = 0 , int c = 0){
		len = a , achi = b , vis = c;
	}
}dp[maxn][maxn];

vector<int> v;
int vsize;

bool cmp(int a , int b){
	return a < b;
}

void init(){
	for(int i = 0;i < maxn;i++){
		for(int j = 0;j < maxn;j++){
			dp[i][j] = Dp(0,0,0);
		}
	}
	v.clear();
	v.push_back(0);
	vsize = 0;
}

void readcase(){
	int visited[maxn] = {0};
	string str;
	getline(cin  , str);
	for(int i = 0;i < str.length();i++){
		if(str[i] <= 'Z' && str[i] >= 'A' && visited[str[i]-'A'] == 0){
			visited[str[i]-'A'] = 1;
			v.push_back(str[i]-'A'+1);
			//cout << v[v.size()-1] << endl;
		}
	}
	sort(v.begin() , v.end() , cmp);
}

Dp DP(int k , int from){
	if(from >= vsize - 1) return Dp(0 , 1 , 1);
	if(dp[k][from].vis == 1) return dp[k][from];
	Dp ans(0,1,1) , tem;
	for(int i = from+1;i < vsize;i++){
		if(v[from]*5 <= v[i]*4){
			tem = DP(k+1 , i);
			if(tem.len+1 > ans.len){
				ans.len = tem.len+1;
				ans.achi = tem.achi;
			}else if(ans.len == tem.len+1){
				ans.achi += tem.achi;
			}
		}
	}
	return dp[k][from] = ans;
}

int main(){
	freopen("in" , "r" , stdin);
	int n;
	cin >> n;
	getchar();
	while(n--){
		init();     //初始化
		int visited[maxn] = {0};
        string str;
        //輸入測資
        getline(cin  , str);
        for(int i = 0;i < str.length();i++){
            if(str[i] <= 'Z' && str[i] >= 'A' && visited[str[i]-'A'] == 0){
                visited[str[i]-'A'] = 1;
                v.push_back(str[i]-'A'+1);
            }
        }
        //執行算法
	    sort(v.begin() , v.end() , cmp);
		vsize = v.size();
        Dp ans = DP(1 , 0);
        cout << ans.len << ' ' << ans.achi << '\n';
	}
}
```