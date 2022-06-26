---
tags: UVA
---

# UVA 10182 - Bee Maja ([Link](https://onlinejudge.org/external/101/10182.pdf))
#數學

## 想法
第1層有1個
第2層有6*1=6個
第3層有6*2=12個
...
第n層有6*(n-1)個

先第幾層再模擬一遍即可

## Code
```c=
#include <iostream>
#include <stack>
#include <string.h>
#define MAXN 200005
#define INF 0x3f3f3f3f3f3f
//#define int long long
using namespace std;

int px, py;
void v1()
{
	px--;
	py++;
}
void v2()
{
	px--;
}
void v3()
{
	py--;
} 
void v4()
{
	px++;
	py--;
}
void v5()
{
	px++;
}
void v6()
{
	py++;
}
int main()
{
	int n, sum, pw, t;
	while(cin>>n)
	{
		sum=0, pw = 0;
        while(sum<n){
			pw++;
			sum+=pw*6;		
		}
		
		sum-=pw*6;	
		n=n-sum;
		if(n==1)
            cout << 0 << ' ' << pw - 1 << '\n';
        else{
			px=pw;
			py=0;
			n--;
			t=pw;
			while(n && t)
			{
				n--;t--;
				v1();
			}
				
			t=pw;
			while(n && t)
			{
				n--;t--;
				v2();
			}
				
			t=pw;
			while(n && t)
			{
				n--;t--;
				v3();
			}
				
			t=pw;
			while(n && t)
			{
				n--;t--;
				v4();
			}
				
			t=pw;
			while(n && t)
			{
				n--;t--;
				v5();
			}
				
			t=pw;
			while(n && t)
			{
				n--;t--;
				v6();
			}

            cout << px << ' ' << py << '\n';
        }
	}
}
```