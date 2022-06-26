---
tags: UVA
---
# UVA 11659 - Informants
#二進位 #爆搜
## 想法
用二進位的方式記錄一群人每一個人提供的資訊
Ex: 一群人包含1,3,4 → 紀錄為1101=13
    1信任2,4 → 紀錄為good[1]=1010=10
再利用位元運算判斷

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
using namespace std;

int good[25], bad[25];      //good[i]=i認為可以信任的人, bad[i]=i認為不可信任的人

int len(int comb){          //計算某一群人的人數
    int cnt = 0;
    while(comb){
        if((comb&1)==1){
            cnt++;
        }
        comb >>= 1;
    }
    return cnt;
}

int32_t main(){
    int a, i;                                   
    while(cin>>a>>i && (a+i!=0)){
        memset(good, 0, sizeof(good));          //歸零
        memset(bad, 0, sizeof(bad));
        int x, y;
        for (int t = 0; t < i; t++){            //記錄每個線民信任與不信任的人
            cin >> x >> y;
            if(y>0){
                good[x] |= (1 << (y - 1));      //用位元或才不會影響到以前的數據
            }
            if(y<0){
                bad[x] |= (1 << (-y - 1));
            }
        }
        int ans=0;  
        int limit = 1 << a;                     //comb二進位中每一位都=1再+1
        for (int t = 0; t<limit; t++){
            int flag=1;                         //此comb合不合法
            int comb=t;
            for(int k=1;k<=a;k++){
                int k_in_comb = comb & (1 << (k - 1));          //k有沒有在comb
                int is_wrong = ((comb & bad[k])) || ((comb & good[k])!=good[k]); 
                //若k在，是不是錯誤的(k不信任的人在內 or k信任的人不都在裡面)
                if(k_in_comb && is_wrong){
                    flag = 0;
                    break;
                }
            }
            if(flag){
                int l = len(comb);
                if(l>ans){
                    ans = l;
                }
            }
        }
        cout << ans << '\n';
    }
}
```