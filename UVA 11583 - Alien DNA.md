---
tags: UVA
---
# UVA 11583 - Alien DNA
#二進位

## 想法
用pre(從上一次切割以來DNA的交集)和(紀錄當前DNA的字母)比較
若pre和cur有交集，則更新pre = pre & cur
若沒有，則需要多切一刀(ans++)，並將pre重置為cur

## Code
```c=
#include <iostream>
#include <string>
#define int long long
using namespace std;

string s;
int t, n;

int32_t main(void){
    cin >> t;
    while(t--){
        cin >> n;
        int pre = (1 << 27) - 1;    //從上一次切割以來DNA的交集，預設為所有字母都有，這樣才不會災剛開始就切一刀
        int ans = 0;
        for (int i = 1; i <= n; i++){
            int cur = 0;            //紀錄當前DNA的字母
            cin >> s;
            //完成cur
            for (int j = 0; j < s.size(); j++){
                cur |= (1 << (s[j] - 'a'));
            }
            //若pre和cur有交集，則更新pre
            if(pre & cur){
                pre = pre & cur;
            }
            //若沒有，則需要多切一刀(ans++)，並將pre重置為cur
            else{
                ans++;
                pre = cur;
            }
        }
        cout << ans << '\n';
    }
}
```