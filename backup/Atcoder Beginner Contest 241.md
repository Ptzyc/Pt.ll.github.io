# 在 Print 之前
这是 2.20 打的一场 VP 也是非常感人，~~第四题磕了一个多小时没磕出来~~，最后终于是过了6道题。
# A - Digit Machine
[题目](https://www.luogu.com.cn/problem/AT_abc241_a)

此题也是**非常**的简单，总共 $3$ 次操作，每次修改一下 $ans$ 即可。
## A - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e2+5;
ll a[N],ans;
int main() {
    for(ll i=0;i<=9;i++) {
        cin>>a[i];
    }
    ans=a[0];
    ans=a[ans];
    ans=a[ans];
    cout<<ans;
    return 0;
}
```
# B - Pasta
[题目](https://www.luogu.com.cn/problem/AT_abc241_b)

此题更简单，用**标记数组**标记即可，但 $a_{i}$ 和 $b_{i}$ 值过大，所以建议用 $map$ 。
## B - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e3+5;
ll n,m,a[N],b[N];
map<ll,ll> mp;
string s="Yes";
int main() {
    cin>>n>>m;
    for(ll i=1;i<=n;i++) {
        cin>>a[i];
        mp[a[i]]++;
    }
    for(ll j=1;j<=m;j++) {
        cin>>b[j];
        if(mp[b[j]]<=0) {
            s="No";
            break;
        }
        mp[b[j]]--;
    }
    cout<<s;
    return 0;
}
```
# C - Connect 6
[题目](https://www.luogu.com.cn/problem/AT_abc241_c)

题目说要找六个竖着、横着、左下斜着以及右下斜着的连续的 `#` 有两次修改机会。

这很明显是一个 **模拟+dfs**。

那么，我们可以思考这样一个问题：
>我们需要往回搜吗(往左、上、右上、左上)？

不难发现，我们上面的情况是已经搜过的了，没必要再次枚举，所以直接往下搜即可。

~~结合代码可能会更好理解~~ 。
## C - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e3+5;
ll n,cnt;
string s;
bool a[N][N],flag;
void dfs(ll x,ll y,ll num,ll k) {
    bool f=0;
    if(k==0&&a[x][y]==0) return ;
    if(a[x][y]==0) {
        k--; 
        a[x][y]=1;
        f=1;
    }
    cnt++;
    if(cnt>=6) return ;
    if(num==1) {
        if(x<=n&&y+1<=n&&x>=1&&y+1>=1) dfs(x,y+1,num,k);
    }
    if(num==2) {
        if(x+1<=n&&y<=n&&x+1>=1&&y>=1) dfs(x+1,y,num,k);
    }
    if(num==3) {
        if(x+1<=n&&y-1<=n&&x+1>=1&&y-1>=1) dfs(x+1,y-1,num,k);
    }
    if(num==4) {
        if(x+1<=n&&y+1<=n&&x+1>=1&&y+1>=1) dfs(x+1,y+1,num,k);
    }
    if(f==1) {
        a[x][y]=0;
    }
}
int main() {
    cin>>n;
    for(ll i=1;i<=n;i++) {
        cin>>s;
        for(ll j=0;j<n;j++) {
            a[i][j+1]=(s[j]=='#');
        }
    }
    for(ll i=1;i<=n;i++) {
        for(ll j=1;j<=n;j++) {
            if(flag==1) break;
            if(a[i][j]!=0) {
                for(ll k=1;k<=4;k++) {
                    cnt=0;
                    dfs(i,j,k,2);
                    if(cnt>=6) {
                        flag=1;
                        break;
                    }
                }
                continue;
            }
            a[i][j]=1;
            for(ll k=1;k<=4;k++) {
                cnt=0;
                dfs(i,j,k,1);
                if(cnt>=6) {
                    flag=1;
                    break;
                }
            }
            a[i][j]=0;
        }
    }
    if(flag==1) {
        cout<<"Yes";
    }
    else {
        cout<<"No";
    }
    return 0;
}
```
# D - Sequence Query
[题目](https://www.luogu.com.cn/problem/AT_abc241_d)

这题第一眼看时，就是个**大模拟+二分**，但如果 $sort$ 的话复杂度来到 $O(Qnlogn)$,~~包超时的~~ 。

那么如何下手呢。

我们一看数据 ：
>$1\le Q\le2\times10^5$。
>
>$1\le x\le10^{18}$。
>
>$1\le k\le5$。

只有个非常不合群的 $k\le5$，那么我们想办法优化 $sort$，扩大查询复杂度不变 $O(log n)$，不就行了。
~~这时只要上网一查~~ ，$multiset$ 好东西，自动排序，复杂度 $O(1)$，这就是为这题量身定做的，就是要思考一下查询：

>当 $num=2$ 时，我们需要输出 $A$ 中所有 $\le x$ 的元素中的第 $k$ 大值。如果不存在输出 `-1`。
>>这只需要搜到**第一个 $>x$ 的数**开始往后**编译 $k$ 次**，如果 `it=a.begin()` 时说明无解，否则输出 $it$。
>
>当 $num=3$ 时，我们需要输出 $A$ 中所有 $\ge x$ 的元素中的第 $k$ 小值。如果不存在输出 `-1`。
>>这只需要从**第一个 $\ge x$ 的数**开始遍历，总共**遍历 $k-1$ 次**，如果 `it=a.end()` 时说明无解，否则输出 $it$。

## D - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e5+5;
ll T,f=0;
multiset<ll> a;
int main() {
    cin>>T;
    while(T--) {
        ll num,x,k;
        cin>>num>>x;
        if(num==1) {
            a.insert(x);
        }
        else {
            cin>>k;
            multiset<ll>::iterator cnt=a.lower_bound(x);
            multiset<ll>::iterator ans=a.upper_bound(x);
            if(num==2) {
                ll op=0;
                auto it=ans;
                for(ll i=1;i<=k;i++) {
                    if(it==a.begin()) {
                        op=1;
                        break;
                    }
                    it--;
                }
                if(op==1) {
                    cout<<-1;
                }
                else {
                    cout<<*it;
                }
            }
            else {
                ll op=0;
                auto it=cnt;
                if(it==a.end()) op=1;
                else {
                    for(ll i=1;i<k;i++) {
                        it++;
                        if(it==a.end()) {
                            op=1;
                            break;
                        }
                    }
                }
                if(op==1) {
                    cout<<-1;
                }
                else {
                    cout<<*it;
                }
            }
            cout<<endl;
        }
    }
    return 0;
}
```
# E - Putting Candies
[题目](https://www.luogu.com.cn/problem/AT_abc241_e)

这是一个很明显的**抽屉原理**，它在查找一定次数后，就会陷入循环，这样我们只需要遍历 $k$ $mod$ $n$ 次就可以了。
## E - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e5+5;
ll a[N],sum[N],num[N],n,t,s,k;
int main() {
	cin>>n>>k;
	for(ll i=0;i<n;i++) {
		cin>>a[i];
    }
	for(ll i=1;i<n;i++) {
        num[i]=-1;
    }
	for(ll i=0;i<n;i++) {
		sum[i+1]=sum[i]+a[sum[i]%n];
		if(num[sum[i+1]%n]!=-1) {
			s=num[sum[i+1]%n];
			t=i+1;
			break;
		}
		num[sum[i+1]%n]=i+1;
	}
	if(k<=s) cout<<sum[k];
	else {
		ll p=t-s;
		ll x=sum[t]-sum[s],pos=k-s-1;
		cout<<sum[s+pos%p+1]+pos/p*x;
	}
	return 0;
}
```
# F - Skate
[题目](https://www.luogu.com.cn/problem/AT_abc241_f)
我借鉴了一下[大佬的题解及代码](https://www.luogu.com.cn/article/rpvqcwg0)~~(我的代码太丑了)~~ 。

这是一道一眼的 $bfs$ 但单单是搜索就很容易爆 $TLE$。

于是我们可以~~很容易~~ 想到用 $set$ 记录下每一行和每一列 分别有哪些障碍，这样向四个方向移动是就可以直接二分出该方向最近的障碍即可，可以使用 `lower_bound`。

记录每一行和每一列分别有哪些障碍和从起点到各个点的距离等数据时，若是直接用数组存，显然直接 $MLE$ 炸掉。我们可以用 $map$ 代替数组存数据。
## F - Code
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
# 后记
总体来说难度适中，不算太难但也不在舒适圈内，~~希望下次有进步~~ 。