[题目](https://codeforces.com/problemset/problem/2093/E)
# 思路
对于 $u=\operatorname{MEX}(v)$，如果选择数组 $v$ 的一部分组成新数组 $s$ 那么在所有的 $w<u$ 中，能否都能找到 $w=\operatorname{MEX}(s)$？

这当然是肯定的，不难想到，我们可以考虑**二分**，下限 $l = 0$，上限 $r=n$（因为数组顶多是 $[0,1, \dots, n-1]$）。

但如何写 $\operatorname{check}$ 函数呢？

我们发现对于 $mx=mid$ 我们只需维护一个区间，然后遍历整个数组，对于每个元素，满足 $a_{i} \le u$ 就将它加入集合，当集合元素个数达到了 $mid$，就统计次数加一，并且清空当前集合。这样到最后，只要计数大于等于 $k$，就表示可以合理划分。时间复杂度为 $O(n \log n)$。

# Code
```cpp
#include<bits/stdc++.h>
#define ll long long
#define inf 2e18
const ll mod=1e9+7;
namespace io {
	using namespace std;
	inline ll read() {
		char n=getchar();
		ll num=0,flag=1;
		while(n<'0'||n>'9') {
			if(n=='-') {
				flag=-1;
			}
			n=getchar();
		}
		while(n>='0'&&n<='9') {
			num=num*10+n-'0';
			n=getchar();
		}
		return num*flag;
	}
	inline void print(ll x) {
		if(x<0) {
			putchar('-');
			print(-x);
			return;
		}
		if(x==0) {
			return;
		}
		print(x/10);
		putchar((char)(x%10+'0'));
	}
}
using namespace io;
const ll N=2e5+5;
ll T,n,k,a[N];
map<ll,bool> mp;
bool ck(ll x) {
	for(ll i=0;i<=x;i++) {
		mp[i]=0;
	}
	ll cnt=0,pos=0;
	for(ll i=1;i<=n;i++) {
		if(a[i]<x&&!mp[a[i]]) {
			mp[a[i]]=1;
			pos++;
		}
		if(pos>=x) {
			cnt++;
			pos=0;
			for(ll j=0;j<=x;j++) {
				mp[j]=0;
			}
		}
	}
	if(cnt>=k) {
		return true;
	}
	return false;
}
void solve() {  
    n=read();
	k=read();
	for(ll i=1;i<=n;i++) {
		a[i]=read();
	}
	ll l=0,r=n,ans=0;
	while(l<=r) {
		ll mid=(l+r)>>1;
		if(ck(mid)) {
			l=mid+1;
			ans=mid;
		}
		else {
			r=mid-1;
		}
	}
	if(!ans) putchar('0');
	else print(ans);
	cout<<endl;
}
int main() {
	T=read();
	while(T--) {
		solve();
	}
	return 0;
}
```

~~当你把这代码交上去时会得到一个令人开心的 TLE~~

为什么呢?

我们发现，`mp.clear()` 的复杂度是 $O(n)$ 的，这样你的复杂度就来到了 $O(n^2 \log n)$。

但我们只需改成普通数组即可光速通过，以下是正确代码：
```cpp
#include<bits/stdc++.h>
#define ll long long
#define inf 2e18
const ll mod=1e9+7;
namespace io {
	using namespace std;
	inline ll read() {
		char n=getchar();
		ll num=0,flag=1;
		while(n<'0'||n>'9') {
			if(n=='-') {
				flag=-1;
			}
			n=getchar();
		}
		while(n>='0'&&n<='9') {
			num=num*10+n-'0';
			n=getchar();
		}
		return num*flag;
	}
	inline void print(ll x) {
		if(x<0) {
			putchar('-');
			print(-x);
			return;
		}
		if(x==0) {
			return;
		}
		print(x/10);
		putchar((char)(x%10+'0'));
	}
}
using namespace io;
const ll N=2e5+5;
ll T,n,k,a[N],mp[N];
bool ck(ll x) {
	for(ll i=0;i<=x;i++) {
		mp[i]=0;
	}
	ll cnt=0,pos=0;
	for(ll i=1;i<=n;i++) {
		if(a[i]<x&&!mp[a[i]]) {
			mp[a[i]]=1;
			pos++;
		}
		if(pos>=x) {
			cnt++;
			pos=0;
			for(ll j=0;j<=x;j++) {
				mp[j]=0;
			}
		}
	}
	if(cnt>=k) {
		return true;
	}
	return false;
}
void solve() {  
    n=read();
	k=read();
	for(ll i=1;i<=n;i++) {
		a[i]=read();
	}
	ll l=0,r=n,ans=0;
	while(l<r) {
		ll mid=(l+r+1)>>1;
		if(ck(mid)) {
			l=mid;
			ans=mid;
		}
		else {
			r=mid-1;
		}
	}
	if(!ans) putchar('0');
	else print(ans);
	putchar('\n');
}
int main() {
	T=read();
	while(T--) {
		solve();
	}
	return 0;
}
```