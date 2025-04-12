# 在 Print 之前
又是熟悉的 $Atcoder$ 的题解，又是一场熟悉的 $VP$，~~又是熟悉的五道题~~

考试时又不知道在干嘛，~~第一题都错了一次~~， $50min$ 时拿下第五题，但第六题思考了 $50min$ 也没拿下...

~~只是没想到前四题都是红题~~。
# A - Last Letter
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_a)

没啥好说的，一如既往的输入输出即可
## A - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=5e6+5;
ll n;
string s;
int main() {
    cin>>n>>s;
    cout<<s[n-1];
    return 0;
}
```
# B - Go Straight and Turn Right
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_b)

也是一道非常简单的**模拟**，用一个 $flag$ 记录当前的方向即可。
## B - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=5e6+5;
ll n,xl,yl,fl;
string s;
void add() {
    if(fl==0) {
        xl++;
    }
    if(fl==1) {
        yl--;
    }
    if(fl==2) {
        xl--;
    }
    if(fl==3) {
        yl++;
    }
}
void num() {
    fl++;
    fl%=4;
}
int main() {
    cin>>n>>s;
    for(ll i=0;i<s.size();i++) {
        if(s[i]=='S') {
            add();
        }
        else {
            num();
        }
    }
    cout<<xl<<' '<<yl;
    return 0;
}
```
# C - Yamanote Line Game
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_c)

~~原本不知道什么是交互式输入输出，结果和平常的没什么不一样~~。

这题最容易想到的思路就是拿一个标记数组来标记用过的数，但当你自信满满的提交上去时，你会得到 $Atcoder$ 独特的 $TLE$。

我们发现最耗时的就是查找的过程，那么我们可以想到用一个 $set$ 存下所有可以打印的数。

每次打印完或输入完就把这个数从 $set$ 中删除即可。

时间复杂度： $O(n \log n)$。
## C - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e3+5;
ll n;
set<ll> st;
int main() {
    cin>>n;
    for(ll i=1;i<=2*n+1;i++) {
        st.insert(i);
    }
    while(1) {
        cout<<*st.begin()<<endl;
        st.erase(st.begin());
        ll x;
        cin>>x;
        if(x==0) return 0;
        st.erase(x);
    }
    return 0;
}
```
# D - Swap Hats
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_d)

第一反应当然是大模拟，但看到 $10^{18}$ 时立刻劝退了。

虽然不能模拟，我们可以讨论交换次数的奇偶性：

1. 初始状态和最终状态相同，那么可以通过不断交换 $1$， $2$ 两个人的帽子保持偶数次操作时状态不变。(所以操作次数为偶数)

2. 有两顶帽子不同，那么可以交换这两顶帽子，然后重复 $1$。(操作次数为奇数)

3. 有三顶帽子不同，那么可以两次操作变成情况 $1$，重复1即可(操作次数为偶数)

操作次数为奇数，显然是 $NO$。

操作次数为偶数，显然是 $YES$。
## D - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 2e18
using namespace std;
const ll N=2e3+5;
char s1,s2,s3,t1,t2,t3;
ll cnt;
int main() {
    cin>>s1>>s2>>s3>>t1>>t2>>t3;
    if(s1!=t1) cnt++;
    if(s2!=t2) cnt++;
    if(s3!=t3) cnt++;
    if(cnt==2) cout<<"No";
    else cout<<"Yes";
    return 0;
}
```
# E - King Bombee
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_e)

题目想让我们记录路径，开头结尾不变，满足无后效性，这很 $dp$。

我们尝试动态规划：

那么我们设 $dp[i][j][k]$ 存的是方案数：

1. $i$ 表示的是当前已经经过的点数；
2. $j$ 表示的是当前路径的最后一个点是 $j$；
3. $k$ 表示的是这个路径中经过X次数的奇偶性($0$ 为偶数， $1$ 为奇数)；

那么初始状态为 $dp[1][S][0]=1$。

答案则为 $dp[k+1][T][0]$。

状态转移为($u$ 为 $j$ 相邻的点)：

> $u==x$
>
> $dp[i][j][0]=dp[i][j][0]+dp[i−1][u][1];$
>
> $dp[i][j][1]=dp[i][j][1]+dp[i−1][u][0];$

> $u \not= x:$
>
> $dp[i][j][0]=dp[i][j][0]+dp[i−1][u][0];$
>
> $dp[i][j][1]=dp[i][j][1]+dp[i−1][u][1];$
## E - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 998244353
#define inf 2e18
using namespace std;
const ll N=2e3+5;
ll n,m,k,s,t,x,dp[N][N][5];
vector<ll> v[N];
int main() {
    cin>>n>>m>>k>>s>>t>>x;
    for(ll i=1;i<=m;i++) {
        ll ul,vl;
        cin>>ul>>vl;
        v[ul].push_back(vl);
        v[vl].push_back(ul);
    }
    dp[0][s][0]=1;
    for(ll i=1;i<=k;i++) {
        for(ll j=1;j<=n;j++) {
            if(!dp[i-1][j][0]&&!dp[i-1][j][1]) continue;
            for(ll l=0;l<v[j].size();l++) {
                ll ul=v[j][l];
                if(ul==x) {
                    dp[i][ul][0]+=dp[i-1][j][1];
                    dp[i][ul][1]+=dp[i-1][j][0];
                }
                else {
                    dp[i][ul][0]+=dp[i-1][j][0];
                    dp[i][ul][1]+=dp[i-1][j][1];
                }
                dp[i][ul][0]%=mod;
                dp[i][ul][1]%=mod;
            }
        }
    }
    cout<<dp[k][t][0];
    return 0;
}
```
# F - Shortest Good Path
[题目](https://atcoder.jp/contests/abc244/tasks/abc244_f)

有题目所给范围以及信息已知本题需要用状态压缩

设 dp[i][j] : 

dp值为路径最小长度

1. $i$ 表示状态，第 $k$ 位为0则代表点 $k$ 出现次数为偶数，反之为奇数；

2. $j$ 表示当前状态下当前所在点为 $j$；

那么显然，所有点出现次数为偶数时，路径长最小值为 $0$。

当只有某个点出现次数为奇数时，路径最小长度为 $1$。

我们可以这些点为出发点，使用 $BFS$，得到所有情况的最小值。

答案可以表示为：

$$
\sum_{(1≪n)−1}^{i=1} min(\sum_{n}^{j=1} dp[i][j])
$$

## F - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 1e9+7
#define inf 0x3f3f3f3f
using namespace std;
const ll N=20;
ll n,m,dp[1<<N][N],lt=-1,hd;
struct node {
    ll x,wl;
}num[N*(1<<N)];
vector<ll> v[N*(1<<N)];
void solve() {
    while(lt>=hd) {
        node d=num[hd];
        hd++;
        for(ll i=0;i<v[d.x].size();i++) {
            ll ul=v[d.x][i];
            ll sum=d.wl^(1<<(ul-1));
            if(dp[sum][ul]==dp[0][0]) {
                dp[sum][ul]=dp[d.wl][d.x]+1;
                num[++lt]={ul,sum};
            }
        }
    }
}
int main() {
    memset(dp,0x3f,sizeof(dp));
    cin>>n>>m;
    for(ll i=1;i<=m;i++) {
        ll ul,vl;
        cin>>ul>>vl;
        v[ul].push_back(vl);
        v[vl].push_back(ul);
    }
    dp[0][1]=0;
    for(ll i=0;i<n;i++) {
        ll x=(1<<i);
        dp[x][i+1]=1;
        num[++lt]={i+1,x};
    }
    solve();
    ll ans=0;
    for(ll i=0;i<(1<<n);i++) {
        ll cnt=inf;
        for(ll j=1;j<=n;j++) {
            cnt=min(cnt,dp[i][j]);
        }
        ans+=cnt;
    }
    cout<<ans;
    return 0;
}
```