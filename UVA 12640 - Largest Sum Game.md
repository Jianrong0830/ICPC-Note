---
tags: UVA
---
# UVA 12640 - Largest Sum Game
#Greedy
## 想法
從頭遍歷一遍，紀錄當前的總和，若當前總和>最大總和，則更新最大總和

另外，若當前總和<0，則重設為0(放棄當前區段，從下一個繼續)，因為這個區段不要選還比較好

輸入沒給n真的很麻煩

## Code
```c=
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#define int long long
#define maxn 100020

using namespace std;

vector<int> a;

int32_t main(void){
    string s;
    stringstream ss;
    //麻煩的輸入
    while(getline(cin,s)){
        a.clear();
        ss.clear();
        ss.str("");
        ss << s;
        int temp;
        int sum = 0, max_sum = 0;
        while(true){
            ss >> temp;
            if((ss.fail()))
                break;
            a.push_back(temp);
        }
        //從頭遍歷一遍，紀錄當前的總和，若當前總和>最大總和，則更新最大總和
        //另外，若當前總和<0，則重設為0(放棄當前區段，從下一個繼續)，因為這個區段不要選還比較好
        
        for(int i:a){
            sum += i;
            if(sum<0)
                sum = 0;
            max_sum = max(sum, max_sum);
        }
        cout << max_sum << '\n';
    }
}
```