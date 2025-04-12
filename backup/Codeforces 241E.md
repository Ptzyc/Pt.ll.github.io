# 题目
[题目传送门](https://codeforces.com/problemset/problem/241/E)
# 大意
有一个 $n$ 个结点且每条边权值均为 $1$ 的有向图，要你把一些边权改成 $2$ 使得任意一条从 $1$ 到 $n$ 的路径长度都相等。
# 分析
由题意可以看出，本题相当于给定了一个有向图，现在让你给每条边确定一个权值。

并且权值的范围在 $1$ 和 $2$ 之间，每条在 $1$ 到 $n$ 之间的路径都为最短路。

那么我们可以令 $dis_{x}$ 为 $x$ 到 $n$ 的最短路，对于每一条边 $a_{u,v}$ 都有

$$
dis_{v+1} \le dis_{u} \le dis_{v+2}
$$

我们~~不难~~看出，这很像一道**差分约束**，那么我们就可以用此方程建立**不等式组**。

因为数据很小 

$$
2 \le n \le 1000
$$ 

$$
1 \le m \le 5000
$$

所以我用的是 `Bellman_Ford` ，~~主要是不想写太多~~。
# Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=5e3+5;
ll n,m,dis[N],a[N][2],vis[N],num[N][2];
void dfs(ll u,ll t) {
	num[u][t]=1;
	for (ll i=1;i<=m;i++) {
        if (a[i][t]==u&&!num[a[i][t^1]][t]) {
            dfs(a[i][t^1],t);
        }
    }
}
void bellman_ford() {
    for(ll i=1;i<=n;i++) {
		for(ll i=1;i<=m;i++) {
            if (vis[a[i][0]]&&vis[a[i][1]]) {
				ll u=a[i][0],v=a[i][1];
				dis[u]=min(dis[v]+2,dis[u]);
				dis[v]=min(dis[u]-1,dis[v]);
			}
        }
    }
}
int main() {
	cin>>n>>m;
	for(ll i=1;i<=m;i++) {
        ll x,y;
        cin>>x>>y;
        a[i][0]=x;
        a[i][1]=y;
    }
	dfs(1,0);
    dfs(n,1);
	for(ll i=1;i<=n;i++) {
        vis[i]=(num[i][0]&&num[i][1]?1:0);
    }
	bellman_ford();
	for(ll i=1;i<=m;i++) {
        if(vis[a[i][0]]&&vis[a[i][1]]) {
			ll x=dis[a[i][0]]-dis[a[i][1]];
			if(x!=1&&x!=2) {
                cout<<"No";
                return 0;
            }
		}
    }
	cout<<"Yes"<<endl;
	for(ll i=1;i<=m;i++) {
        cout<<max(min(dis[a[i][0]]-dis[a[i][1]],2ll),1ll)<<endl;
    }
	return 0;
}
```