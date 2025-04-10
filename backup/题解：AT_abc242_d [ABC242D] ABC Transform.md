# D - ABC Transform
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_d)

我们不难发现，它每次分裂出两个节点，这是啥，二叉树啊，请看图：
![](https://cdn.luogu.com.cn/upload/image_hosting/uq13mj8o.png)

所以我们只需用一个变量统计一下单复数，左根加 $1$ 右根加 $2$，即：

$$sum = sum + \begin{cases}
1 & k \equiv 1 \pmod 2\\
2 & k \equiv 0 \pmod 2
\end{cases}$$

注意，如果 $60 \le t$ 那么数肯定大于 $10^{18}$。
# D - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 998244353
using namespace std;
const ll N=1e6+5;
ll q;
string s;
char ans;
int main() {
    cin>>s>>q;
    while(q--) {
        ll k,t;
        cin>>t>>k;
        ll sum=0;
        while(1) {
            if(!t||k==1) break;
            if(k%2==1) sum++;
            else sum+=2;
            k=(k+1)/2;
            t--;
        }
        if(t) {
            ans=(s[0]-'A'+sum+t)%3+'A';
        }
        else {
            ans=(s[k-1]-'A'+sum)%3+'A';
        }
        cout<<ans<<endl;
    }
    return 0;
}
```