---
tags: UVA
---

# UVA 10022 - Delta-wave ([Link](https://onlinejudge.org/external/100/10022.pdf))
#數學

## 想法
先轉換成座標，並分類討論
1.朝上三角形到朝上三角形
2.朝上三角形到朝下三角形
3.朝下三角形到朝上三角形
4.朝下三角形到朝下三角形
3、4都先把m平移成1、2的狀況再討論
1、2若n在m延伸的大三角型中則可歸納出公式，若不再則平移到大三角形內

## Code
```c=
#include <iostream>
#include <algorithm>
#include <string.h>
#define MAXN 200005
#define INF 0x3f3f3f3f3f3f
#define int long long
using namespace std;

//轉成座標
pair<int,int> getCord(int num){
    int temp = 1;
    int x = 1, y;
    while(num>temp){
        num -= temp;
        temp += 2;
        x++;
    }
    y = num;
    return {x, y};
}

int t;
int m, n;

int32_t main(void){
    cin >> t;
    while(t--){
        cin >> m >> n;
        //若從下面開始則交換
        if(m>n){
            int temp = m;
            m = n;
            n = temp;
        }
        pair<int, int> mc = getCord(m), nc = getCord(n);
        int mx = mc.first, my = mc.second, nx = nc.first, ny = nc.second;
        if(mx==nx){
            cout << abs(my - ny) << "\n\n";
            continue;
        }
        //從正
        int ans = INF;
        if(my%2==1){
            for (int i = my; i <= my + 2 * (nx - mx); i++) {
                //到正
                if(i%2==1){
                    ans = min(ans, 2 * (nx - mx) + abs(i - ny));
                }
                //到反
                else{
                    ans = min(ans, 2 * (nx - mx) + abs(i - ny) - 1);
                }
            }
        }
        //從反
        else{
            for (int i = 1; i <= mx * 2 - 1; i+=2){
                if(i<=ny && ny<=i+2*(nx-mx)){
                    //到正
                    if(ny%2==1){
                        ans = min(ans, 2 * (nx - mx) + abs(i - my));
                    }
                    //到反
                    else{
                        ans = min(ans, 2 * (nx - mx) + abs(i - my) - 1);
                    }
                }
            }
        }
        cout << ans << '\n';
        if(t>=1)
            cout << '\n';
    }
}
```