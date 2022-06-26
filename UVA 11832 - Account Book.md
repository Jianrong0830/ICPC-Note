---
tags: UVA
---
# UVA 11832 - Account Book
#DP #二進位
## 想法
好複雜...
收入和支出分開開DP陣列比較好處理
DP[i][j] i、j分別為明細數量、結餘
紀錄DP內每筆資料是收入還是支出：用二進位
EX：2(110) 第一筆是支出，二、三是收入
## DP邏輯關係(by David)
```c=
int L = 累積交易明細都是支出的結餘
int R = 累積交易明細都是收入的結餘

for(交易明細){
    for(L~R){ //加速搜尋，以避免需要 0~100000 大量 for
        if(收入交易[上一個交易][結餘] || 支出交易[上一個交易][結餘]){
            收入交易[這一個交易][結餘 + 正交易明細] |=  收入交易[上一個交易][結餘] + 正交易明細
            支出交易[這一個交易][結餘 + 正交易明細] |=  收入交易[上一個交易][結餘] 
            收入交易[這一個交易][結餘 + 負交易明細] |=  收入交易[上一個交易][結餘] 
            支出交易[這一個交易][結餘 + 負交易明細] |=  收入交易[上一個交易][結餘] + 負交易明細

            //也就是說當你想看某個結餘時，你必須同時看支出交易、收入交易。
            //支出交易負責 支出明細
            //收入交易負責 收入明細

            //注意必須要是 |=，因為如果剛好另種組合也等於結餘時，必須兩種狀態都保存，所以是 or
        }
    }
}
```

## Code
```c=
#include <iostream>
#include <string.h>
#define int long long
#define clr(k) memset(k,0,sizeof(k))
using namespace std;

int n, f, t;
int add[2][100000], subtract[2][100000];    //收入和支出分開開DP陣列
int num[1005];      //記錄每筆明細

int32_t main(void){
    while(cin>>n>>f && n!=0){
        clr(add); clr(subtract);
        for (int i = 0; i < n; i++)
            cin >> num[i];
        int L = 50000 - num[0], R = 50000 + num[0], roll = 0;       //以50000當作結餘0的標準，往右是收入，左是支出
        add[0][R] = 1, subtract[0][L] = 1;                          //須預先設定
        //建立DP(參考學長寫的邏輯關係)
        for (int i = 1; i < n; i++){
            clr(add[!roll]); clr(subtract[!roll]);
            for (int j = L; j <= R; j++){   //不須從0跑到100000
                if(add[roll][j] || subtract[roll][j]){
                    int add_value = j + num[i];
                    add[!roll][add_value] |= add[roll][j] | (1LL << i);
                    subtract[!roll][add_value] |= subtract[roll][j];
                    int subtract_value = j - num[i];
                    add[!roll][subtract_value] |= add[roll][j];
                    subtract[!roll][subtract_value] |= subtract[roll][j] | (1LL << i);
                }
            }
            L -= num[i];
            R += num[i];
            roll = !roll;
        }
        f += 50000;     //和一開始一樣，須加上50000
        if(add[roll][f] == 0 && subtract[roll][f] == 0){
            cout << "*\n";
            continue;
        }
        for (int i = 0; i < n; i++){
            int item = 1LL << i;        //2進位
            if(add[roll][f] & item)     //收入
                if(subtract[roll][f] & item)    //收入及支出
                    cout << '?';
                else
                    cout << '+';
            else if(subtract[roll][f] & item)   //支出
                cout << '-';
        }
        cout << '\n';
    }
}
```