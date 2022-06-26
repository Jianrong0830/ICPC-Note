---
tags: Algorithm
---
# 二分搜
```c=
#include <iostream>

using namespace std;

int A[10] = {0, 3, 5, 6, 8, 9, 12, 14, 25, 30};

int bsearch(int num, int l, int r){
    int m = (l + r) / 2;
    if(num == A[m])
        return m;
    if(num > A[m])
        return bsearch(num, m + 1, r);
    return bsearch(num, l, m);
}

int main(void){
    cout << bsearch(9,0,9);
}
```