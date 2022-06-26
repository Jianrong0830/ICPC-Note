---
tags: Algorithm
---
# LCS
```c=
#include <iostream>

using namespace std;

int dp[6][6];
int a[6] = {NULL, 3, 5, 4, 7, 6};
int b[6] = {NULL, 2, 3, 5, 4, 6};

int main(void){
    for (int i = 1; i <= 5; i++)
    {
        for (int j = 1; j <= 5; j++)
        {
            if (a[i] == b[j])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else{
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    cout << dp[5][5];
}
```