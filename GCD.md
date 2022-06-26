---
tags: Algorithm
---
# GCD
```c
#include <iostream>

using namespace std;

int gcd(int a, int b){
    if(a==0)
        return b;
    if(b==0)
        return a;
    if(a<b){
        return gcd(a, b % a);
    }
    return gcd(a % b, b);
}

int main(void){
    cout << gcd(546, 429);
}
```