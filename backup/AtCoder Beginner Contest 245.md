# 在 Print 之前
又是 $Atcoder$ 的题解，又是一场 $VP$，~~又是五道题~~

我不知道在比赛时我在干嘛， $T3$ 交了 $6$ 遍才过， $T4$ 交了 $3$ 遍， $T5$ 交了 $5$ 遍才过， $T6$ 因为一点小错误，~~没写出来~~。
# A - Good morning
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_a)

~~鄙人觉得没啥好说的~~。

一道非常简单的红题，只需判断 $Takahashi$ 和 $Aoki$ 谁的时间更早即可。
## A - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e5+5;
ll a,b,c,d;
int main() {
    cin>>a>>b>>c>>d;
    if(a<c) {
        cout<<"Takahashi";
    }
    else if(a==c&&b<=d) {
        cout<<"Takahashi";
    }
    else {
        cout<<"Aoki";
    }
    return 0;
}
```
# B - Mex
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_b)

~~鄙人又觉得没啥好说的~~。

只需一个 $sort$ 带走，当然用 $set$ 可能更快。
## B - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e5+5;
ll n,a[N],ans=0;
int main() {
    cin>>n;
    for(ll i=1;i<=n;i++) {
        cin>>a[i];
    }
    sort(a+1,a+1+n);
    for(ll i=1;i<=n;i++) {
        if(a[i]==ans) ans++;
    }
    cout<<ans;
    return 0;
}
```
# C - Choose Elements
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_c)

我们只需思考上一位的数可以接过来的是 $A$ 还是 $B$ 。

我们可以定义两个数组，一个 $sum$,这个数是由 $A$ 序列产生的， $num$ 则表示是由 $B$ 序列产生的。

那么对于最新一位 $i$ 只需考虑 $i-1$ 这个位置能填数的是 $num_{i-1}$ 或 $sum_{i-1}$ 成立即可。
## C - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e6+5;
ll n,k,a[N],b[N],num[N],sum[N];
string s="Yes";
int main() {
    cin>>n>>k;
    for(ll i=1;i<=n;i++){
        cin>>a[i];
    }
    for(ll i=1;i<=n;i++){
        cin>>b[i];
    }
    num[1]=1;
    sum[1]=1;
    for(ll i=2;i<=n;i++){
        if(sum[i-1]) {
            if(abs(a[i]-a[i-1])<=k) sum[i]=1;
            if(abs(b[i]-a[i-1])<=k) num[i]=1;
        }
        if(num[i-1]) {
            if(abs(b[i]-b[i-1])<=k) num[i]=1;
            if(abs(a[i]-b[i-1])<=k) sum[i]=1;
        }
    }
    for(ll i=2;i<=n;i++){
        if(sum[i]||num[i]) continue;
        s="No";
        break;
    }
    cout<<s;
    return 0;
}
```
# D - Polynomial division
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_d)

我们一直一个乘数，又知道最后的乘积，那么只需考虑 $C_{i}$ 可以由哪些数得来，例如:

$$
C_{n+m-1}=A_{n} \times B_{m-1} + A_{n-1} \times B_{m}
$$

因为 $A_{n}$， $A_{n-1}$ 和 $C_{n+m-1}$ 是已知的，且 $B_{m}$ 可以直接通过 $C_{n+m-1} \div A_{n}$ 得来，所以 $B_{m-1}$ 可以求出来。

同理，剩下的数都可以求出来，所以只需两层循环即可。
## D - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e6+5;
ll n,m,a[N],c[N],b[N];
int main() {
    cin>>n>>m;
    for(ll i=0;i<=n;i++) {
        cin>>a[i];
    }
    for(ll i=0;i<=n+m;i++) {
        cin>>c[i];
    }
    b[m]=c[n+m]/a[n];
    for(ll i=m-1;i>=0;i--) {
        ll cnt=0,sum=0;
        for(ll j=n-1;j>=n-(m-1-i)-1;j--) {
            cnt++;
			sum+=b[i+cnt]*a[j];
        }
        b[i]=(c[n+i]-sum)/a[n];
    }
    for(ll i=0;i<=m;i++) {
        cout<<b[i]<<' ';
    }
    return 0;
}
```
# E - Wrapping Chocolate
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_e)

这题一看就是**贪心**，对于某个物品来说，我们要尽量放到小的箱子里，考虑长和宽两个因素比较困难，因此我们可以对其按长排序，只考虑长，若不存在某个宽度满足条件，则说明无法放入。

我们只需考虑巧克力的长或宽与盒子的长或宽的大小，这只需一个 $sort$。

但在进行贪心时，需要考虑长的大小，并进行排序，还需用**二分**来查找。

毕竟

- $1 \le N \le M \le 2 \times 10^5$

- $1 \le A_i,B_i,C_i,D_i \le 10^9$

所以在具体的实现中，可以使用 $multiset$ 来维护箱子的宽度，然后使用 `lower_bound` 来遍历物品和箱子。

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
# F - Endless Walk
[题目](https://atcoder.jp/contests/abc245/tasks/abc245_f)

~~这么明显的拓扑我居然没调出来~~。

这~~很容易~~想到，所有能够走到环上的点和组成环的点的数量和即是答案。

那么如何去统计这些点的数量呢？ 

并不是很好统计(用 $Tanjan$ 缩点也可以)，正难则反，我们考虑有多少点无法一直走下去。

其实此时就可以想到反向建边了，在反向建边的情况下，考虑有多少个点可以走到环，则这些点实际上就是无法走到环的点。

用**拓扑排序**就可以很好的解决这个问题，有多少点能够入队，那么这些点就是实际上无法走到环的点。
## F - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e6+5;
ll n,m,in[N],ans;
vector<ll> v[N];
queue<ll> q;
void tosort() {
    for(ll i=1;i<=n;i++) {
        if(in[i]==0) q.push(i);
    }
    while(!q.empty()) {
        ll cnt=q.front();
        q.pop();
        ans--;
        for(ll i=0;i<v[cnt].size();i++) {
            ll sum=v[cnt][i];
            in[sum]--;
            if(in[sum]==0) q.push(sum);
        }
    }
}
int main() {
    cin>>n>>m;
    for(ll i=1;i<=m;i++) {
        ll ul,vl;
        cin>>ul>>vl;
        v[vl].push_back(ul);
        in[ul]++;
    }
    ans=n;
    tosort();
    cout<<ans;
    return 0;
}
```