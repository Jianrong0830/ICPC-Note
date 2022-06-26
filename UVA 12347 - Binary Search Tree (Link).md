---
tags: UVA
---
# UVA 12347 - Binary Search Tree ([Link](https://onlinejudge.org/external/123/12347.pdf))
#BST

## 想法
就是棵樹，要弄熟

## Code
```c=
#include <iostream>
using namespace std;

struct node{
    int num = 0;
    node *l = nullptr;
    node *r = nullptr;
};

void insert(int obj, node* &ptr){
    if(ptr==nullptr){
        ptr = new node;
        ptr->num = obj;
        ptr->l = nullptr;
        ptr->r = nullptr;
        return;
    }
    if(obj < ptr->num)
        insert(obj, ptr->l);
    if(obj > ptr->num)
        insert(obj, ptr->r);
}

void post_order(node *root){
    if(root==nullptr)
        return;
    post_order(root->l);
    post_order(root->r);
    cout << root->num << '\n';
}

int32_t main(void){
    node *head = nullptr;
    int cur;
    while(cin>>cur){
        insert(cur, head);
    }
    post_order(head);
}
```