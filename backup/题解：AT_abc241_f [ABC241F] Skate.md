# Skate

[题目](https://www.luogu.com.cn/problem/AT_abc241_f)

这是一道一眼的 **Bfs** 但单单是搜索就很容易爆 **TLE**。

于是我们可以~~很容易~~想到用 `set` 记录下每一行和每一列 分别有哪些障碍，这样向四个方向移动是就可以直接二分出该方向最近的障碍即可，可以使用 `lower_bound`。

记录每一行和每一列分别有哪些障碍和从起点到各个点的距离等数据时，若是直接用数组存，显然直接 **MLE** 炸掉。我们可以用 `map` 代替数组存数据。

# Code

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
ll h,w,n,sx,sy,ex,ey,ans=-1;
map<ll,set<ll> > x,y;
map<pair<ll,ll>,ll> d;
queue<pair<ll,ll> > q;
void bfs() {
    q.push({sx,sy});
	d[{sx,sy}]=0;
	while(!q.empty()) {
        pair<ll,ll> pl=q.front();
		ll u=pl.first,v=pl.second;
		q.pop();
		if(u==ex&&v==ey) {
			ans=d[{u,v}];
			break;
		}
		auto it=x[u].lower_bound(v);
		if(it!=x[u].begin()) {
			if(d.find({u,*prev(it)+1})==d.end()) {
				q.push({u,*prev(it)+1});
				d[{u,*prev(it)+1}]=d[{u,v}]+1; 
			}
		}
		if(it!=x[u].end()) {
			if(d.find({u,*it-1})==d.end()) {
				q.push({u,*it-1});
				d[{u,*it-1}]=d[{u,v}]+1; 
			}
		}
		it=y[v].lower_bound(u);
		if(it!= y[v].begin()) {
			if(d.find({*prev(it)+1,v})==d.end()) {
				q.push({*prev(it)+1,v});
				d[{*prev(it)+1,v}]=d[{u,v}]+1; 
			}
		}
		if(it!=y[v].end()) {
			if(d.find({*it-1,v})==d.end()) {
				q.push({*it-1,v});
				d[{*it-1,v}]=d[{u,v}]+1; 
			}
		}
	}
}
int main() {
	cin>>h>>w>>n>>sx>>sy>>ex>>ey;
	for (ll i = 1; i <= n; i++) {
		ll ul,vl;
		cin>>ul>>vl;
		x[ul].insert(vl);
		y[vl].insert(ul);
	}
    bfs();
	cout<<ans;
	return 0;
}
```