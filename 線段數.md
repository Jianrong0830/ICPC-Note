---
tags: Algorithm
---
# 線段數
```c
#include <iostream>

using namespace std;

int n;
int A[500001]={0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int seg[2000001];
int tag[2000001];

void init(){
    memset(seg,0,sizeof(seg));
    memset(tag,0,sizeof(tag));
}

void push(int id,int l,int r){
    int m = (l + r) / 2;
    if(tag[id] > 0){
        seg[id * 2] += tag[id] * (m - l + 1), seg[id * 2 + 1] += tag[id] * (r - m);
        tag[id * 2] += tag[id], tag[id * 2 + 1] += tag[id];
        tag[id] = 0;
    }
}

void build(int id, int l, int r){
    if(l == r){
        seg[id] = A[l];
        return;
    }
    int m = (l + r) / 2;
    build(id * 2, l, m);
    build(id * 2 + 1, m + 1, r);
    seg[id] = seg[id * 2] + seg[id * 2 + 1];
}

int query(int id, int l, int r, int ql, int qr){
    if(ql > r || qr < l)
        return 0;
    if(ql <= l && qr >= r)
        return seg[id];
    push(id, l, r);
    int m = (l + r) / 2;
    return query(id * 2, l, m, ql, qr) + query(id * 2 + 1, m + 1, r, ql, qr);
}

void update(int id, int l, int r, int ul, int ur, int v){
    if(ul <= l && r <= ur){
        seg[id] += v * (r - l + 1);
        tag[id] += v;
        return;
    }
    push(id, l, r);
    int m = (l + r) / 2;
    if(ul<=m)
        update(id * 2, l, m, ul, ur, v);
    if(ur>m)
        update(id * 2 + 1, m + 1, r, ul, ur, v);
    seg[id] = seg[id * 2] + seg[id * 2 + 1];
}

int main(){
    init();
    build(1, 1, 10);
}
```