# A-Poisonous Oyster
[题目](https://atcoder.jp/contests/abc393/tasks/abc393_a)

本题很简单，因为 *Takahashi* 吃了1和2，而 *Aoki* 吃了1和3，所以他们都为 *fine* 时，4号就有问题；同理，可以判断那个有问题。
## Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e5+5;
string s1,s2; 
int main() {
    cin>>s1>>s2;
    if(s1=="fine") {
    	if(s2=="fine") cout<<4;
    	else cout<<3;
	}
	else {
		if(s2=="fine") cout<<2;
    	else cout<<1;
	}
    return 0;
}
```
# B-A..B..C
[题目](https://atcoder.jp/contests/abc393/tasks/abc393_b)

这是一道典型的三元组，由

$$
j−i=k−j
$$

可得

$$
k=2*j-i
$$

所以两层循环即可，然后判断可行性。
时间复杂度为 $O(n^2)$。
## Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e5+5;
string s; 
ll n,ans;
int main() {
    cin>>s;
    n=s.size();
    for(ll i=0;i<n;i++) {
    	if(s[i]!='A') continue;
    	for(ll j=i+1;j<n;j++) {
    		if(s[j]!='B') continue;
    		ll k=j*2-i;
    		if(k>=n) break;
    		if(s[k]=='C') ans++;
		}
	}
	cout<<ans;
    return 0;
}
```
# C-	Make it Simple
[题目](https://atcoder.jp/contests/abc393/tasks/abc393_c)

题目要满足两个条件：
1. 无自环
2. 无重边

自环很好判，但在判重边时需注意它是无向图。

p.s. 因为数据很大所以建议用 *map*。
## Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e5+5;
ll n,m,ans;
struct node{
	ll ul,vl;
};
map<pair<ll,ll>,bool> mp;
int main() {
    cin>>n>>m;
    for(ll i=1;i<=m;i++) {
    	ll ul,vl;
    	cin>>ul>>vl;
    	if(ul==vl) {
    		ans++;
    		continue;
		}
    	ll x=min(ul,vl),y=max(ul,vl);
    	if(mp[{x,y}]==1) ans++;
    	mp[{x,y}]=1;
	}
	cout<<ans;
    return 0;
}
```
# D-Swap to Gather
[题目](https://atcoder.jp/contests/abc393/tasks/abc393_d)

我们可以用 $a$ 数组来统计之前出现的 $1$ 的个数，即 $a_{i}$ 表示 $i$ 之前 $1$ 的次数，然后我们可以找到最中间的一位 $1$ 或中间两位 $1$ 的中间值令为 $cnt$;因为每次只能交换字符的**上一位和下一位**，所以令

$$
k=|a[cnt]-a[i]|
$$

然后用 $ans=ans+|cnt-i|-k$，最后输出 $ans$ 即可。
时间复杂度为 $O(n)$。
## Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=5e5+5;
ll n,ans,cnt,a[N];
string s;
int main() {
    cin>>n>>s;
    for(ll i=0;i<n;i++) {
    	if(s[i]=='1') {
    		ans++;
		}
    	a[i]=a[i-1]+s[i]-'0';
	}
	if(ans%2==1) {
		for(ll i=0;i<n;i++) {
			if(s[i]=='1') {
				cnt++;
			}
			if(cnt==ans/2+1) {
				cnt=i;
				break;
			}
		}
	}
	else {
		ll lt=0;
		for(ll i=0;i<n;i++) {
			if(s[i]=='1') {
				cnt++;
			}
			if(cnt==ans/2+1) {
				cnt=(i+lt)/2;
				break;
			}
			if(s[i]=='1') {
				lt=i;
			}
		}
	}
	ans=0;
	for(ll i=0;i<n;i++) {
		if(s[i]=='1') {
			ll k=abs(a[cnt]-a[i]);
			if(i<=cnt) {
				ans+=(cnt-i-k);
			}
			else {
				ans+=(i-cnt-k);
			}
		}
	}
	cout<<ans;
    return 0;
}
```
# 后记
T5(E)因翻译没写出来……

~~新学期得好好学英语了~~。