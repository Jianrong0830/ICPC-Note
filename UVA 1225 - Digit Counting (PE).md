---
tags: UVA
---
# UVA 1225 - Digit Counting (PE)
string
```c=
#include <iostream>
#include <string>
using namespace std;

int t, n;

int main(void){
    cin >> t;

    for (int i = 1; i <= t; i++){
        cin >> n;
        string s = "";
        int cnt[10];
        for (int i = 0; i < 10; i++){
            cnt[i] = 0;
        }
        for (int i = 1; i <= n; i++){
            s += to_string(i);
        }
        for (int i = 0; i < s.size(); i++){
            cnt[int(s[i]) - int('0')]++;
        }
        for (int i = 0; i < 10; i++){
            cout << cnt[i] << ' ';
        }
        cout << '\n';
    }
}
```
math
```c=
#include <iostream>
using namespace std;

int t, n;
int cnt[10];

int main(void){
    cin >> t;
    for (int i = 1; i <= t; i++){
        cin >> n;
        for (int j = 0; j < 10; j++){
            cnt[j] = 0;
        }
        for (int j = 1; j <= n; j++){
            int c = j;
            while(c!=0){
                cnt[c % 10]++;
                c = int((c - c % 10) / 10);
            }
        }
        for (int j = 0; j < 10; j++){
            cout << cnt[j] << ' ';
        }
        cout << '\n';
    }
}
```