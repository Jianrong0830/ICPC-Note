---
tags: UVA
---
# UVA 12694 - Meeting Room Arrangement
```c=
#include <iostream>
#include <algorithm>
using namespace std;

int n;
struct activity{
    int s,f;
};

bool cmp(activity a, activity b){
    return a.f<b.f;
}

int main(void){
    cin>>n;
    for(int t=1;t<=n;t++){
        activity A[20];
        int index=0;
        while(cin>>A[index].s>>A[index].f && !(A[index].s==0 && A[index].f==0)){
            index++;
        }
        int cnt=index;
        sort(A,A+cnt,cmp);
        int l=0,ans=0;
        for(int i=0;i<cnt;i++){
            if(l<=A[i].s){
                l=A[i].f;
                ans++;
                //cout << A[i].s << ' ' << A[i].f << endl;
            }
        }
        cout<<ans<<endl;
    }
}
```