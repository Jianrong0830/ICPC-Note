---
tags: UVA
---
# UVA 11470 - Square Sums
```c=
#include <iostream>
using namespace std;

int n;
int map[11][11];
int visited[11][11];

void init(){
    for (int i = 0; i <= 10; i++){
        for (int j = 0; j <= 10; j++){
            map[i][j] = 0;
            visited[i][j] = 0;
        }
    }
}

int main(void){
    int r = 1;
    while(cin>>n && n!=0){
        init();
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= n; j++){
                cin >> map[i][j];
            }
        }
        int x = 1, y = 0;
        int m = int(n / 2) + 1;
        cout << "Case " << r << ": ";
        for (int i = 1; i <= m; i++){
            int sum = 0;
            while(y+1<=n && visited[x][y+1]==0){
                y++;
                sum += map[x][y];
                visited[x][y] = 1;
            }
            while(x+1<=n && visited[x+1][y]==0){
                x++;
                sum += map[x][y];
                visited[x][y] = 1;
            }
            while(y-1>=1 && visited[x][y-1]==0){
                y--;
                sum += map[x][y];
                visited[x][y] = 1;
            }
            while(x-1>=1 && visited[x-1][y]==0){
                x--;
                sum += map[x][y];
                visited[x][y] = 1;
            }
            cout << sum << ' ';
            y++;
        }
        cout << '\n';
        r++;
    }
}
```