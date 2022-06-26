---
tags: Algorithm
---
# Disjoint Set
```c=
#include <iostream>

using namespace std;

int set_parent[7] = {NULL, -1, 3, 1, 1, -1, 5};

int findRoot(int id){
    if(set_parent[id] == -1)
        return id;
    return (set_parent[id] = findRoot(set_parent[id]));
}

void mergeSet(int a,int b){
    int root_a = findRoot(a);
    int root_b = findRoot(b);

    set_parent[root_b] = root_a;
}

int main(void){
    mergeSet(2, 6);
    for (int i = 1; i <= 6; i++)
        cout << set_parent[i] << ' ';
}
```