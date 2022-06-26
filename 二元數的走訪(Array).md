---
tags: Algorithm
---
# 二元數的走訪(Array)
```c=
#include <iostream>

using namespace std;

int btree[8] = {-1, 1, 2, 3, -1, 5, 6, 7};

void Preorder(int id){
    if(id > 7 || btree[id] == -1){
        return;
    }
    cout << id << ' ';
    Preorder(id * 2);
    Preorder(id * 2 + 1);
}

int main(void){
    Preorder(1);
}
```