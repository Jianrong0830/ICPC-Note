---
tags: UVA
---
# UVA 11259	- Coin Changing Again
#DP #排冗原理
## 想法
先視為無限背包問題，不要管硬幣數量，之後再用排冗原理扣除非法組合(硬幣數量超過上限)

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define maxn 100020

using namespace std;

int n;
int c[4], d[4];
int q, v;
int dp[maxn];

int32_t main(void){
    cin >> n;
    while(n--){
        memset(c, 0, sizeof(c));
        memset(d, 0, sizeof(d));
        memset(dp, 0, sizeof(dp));
        for (int i = 0; i < 4; i++){
            cin >> c[i];
        }
        cin >> q;

        //無限背包問題
        //dp[j]=最大金額為j時的組合數
        dp[0] = 1;
        for (int i = 0; i < 4; i++){
            for (int j = c[i]; j < maxn; j++){
                dp[j] += dp[j - c[i]];
            }
        }

        while(q--){
            for (int i = 0; i < 4; i++){
                cin >> d[i];
            }
            cin >> v;

            //排冗原理
            int ans = 0;
            for (int i = 0; i <= 15; i++){      //有16種組合，C4取20一直加到C4取4，用2進位表示，5=0101意思是此組合有硬幣1和2
                int x = v, flag = 1;            //x為v減去非法金額後的值，flag判斷要扣掉還是補回
                for (int j = 0; j < 4; j++){    //4種硬幣
                    if(i & (1<<j)){             //判斷此組合存不存在硬幣j
                        flag *= -1;
                        x -= c[j] * (d[j] + 1);
                    }
                }
                if(x >= 0){                     //x>0代表此非法組合存在
                    ans += flag * dp[x];
                }
            }
            cout << ans << '\n';
        }
    }
}
```