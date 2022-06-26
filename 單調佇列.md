---
tags: Algorithm
---
# 單調佇列

```c
#include <iostream>
#include <deque>

using namespace std;

int A[8] = {5, 3, 4, 0, 8, 1, 9, 2};
int m[8];
deque<int> q, index;

int main(void){
    q.push_back(A[0]);
    index.push_back(0);
    m[0] = A[0];
    for (int i = 0; i < 8; i++){
        while(!q.empty() && A[i]<q.back()){
            q.pop_back();
            index.pop_back();
        }
        q.push_back(A[i]);
        index.push_back(i);
        if(i-index.front()>=3){
            q.pop_front();
            index.pop_front();
        }
        m[i] = q.front();
    }

    for (int i = 0; i < 8; i++){
        cout << m[i] << ' ';
    }
}
```