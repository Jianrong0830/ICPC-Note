---
tags: UVA
---
# UVA 12321 - Gas Stations
#Greedy

## Code
```c=
#include <iostream>
#include <vector>
#include <algorithm>
#define int long long
using namespace std;

int g, l, x, r;
vector<pair<int,int> > gas;

int32_t main()
{
    while(cin >> l >> g, l||g){
        gas.clear();
        for(int i = 1; i <= g; i++){
            cin >> x >> r;
            gas.push_back({x-r, x+r});
        }
        sort(gas.begin(), gas.end());
        int coverage = 0, temp_coverage = 0, eliminated = g, i = 0;
        while(l > coverage){    //如果coverage還沒有把幹道每個點都覆蓋，temp = 右邊界
            temp_coverage = coverage;
            while(i < g && gas[i].first <= coverage){   //右邊界必<=coverage
                if(gas[i].second > temp_coverage){
                    //比temp還長，則延長temp至gas[i].second
                    temp_coverage = gas[i].second;
                }
                i++;
            }
            if(coverage == temp_coverage){  //無法繼續延長
                break;
            }
            coverage = temp_coverage;   //將當前區段延伸到暫時區段
            eliminated--;
        }

        if(coverage < l){
            cout << -1 << '\n';
        }
        else{
            cout << eliminated << '\n';
        }
    }
}
```