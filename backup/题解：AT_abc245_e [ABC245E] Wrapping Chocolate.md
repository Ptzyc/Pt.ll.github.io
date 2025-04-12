# E - Wrapping Chocolate
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_e)

这题一看就是**贪心**，对于某个物品来说，我们要尽量放到小的箱子里。

我们只需考虑巧克力的长或宽与盒子的长或宽的大小，这只需一个 `sort`。

考虑长和宽两个因素比较困难，因此我们可以对其按长排序，只考虑长，若不存在某个宽度满足条件，则说明无法放入。

但在进行贪心时，需要考虑长的大小，并进行排序，还需用**二分**来查找。

毕竟
- $1 \le N \le M \le 2\times\ 10^5$
- $1 \le A_i,B_i,C_i,D_i \le 10^9$

所以在具体的实现中，可以使用 `multiset` 来维护箱子的宽度，然后使用 `lower_bound` 来遍历物品和箱子。

最后记得把用过的数删掉。
## E - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e5+5;
struct node {
    ll x,y;
}a[N],b[N];
ll n,m;
multiset<ll> s;
string str="Yes";
bool cmp(node a,node b) {
    return a.x<b.x;
}
int main() {
    cin>>n>>m;
    for(ll i=1;i<=n;i++){
        cin>>a[i].x;
    }
    for(ll i=1;i<=n;i++) {
        cin>>a[i].y;
    }
    for(ll i=1;i<=m;i++) {
        cin>>b[i].x;
    }
    for(ll i=1;i<=m;i++) {
        cin>>b[i].y;
    }
    sort(a+1,a+1+n,cmp);
    sort(b+1,b+1+m,cmp);
    ll num=m;
    for(ll i=n;i>=1;i--) {
        while(num>=1&&a[i].x<=b[num].x) {
            s.insert(b[num].y);
            num--;
        }
        auto it=s.lower_bound(a[i].y);
        if(it==s.end()) {
            str="No";
            break;
        }
        s.erase(it);
    }
    cout<<str;
    return 0;
}
```