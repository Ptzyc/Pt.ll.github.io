@[Toc](Atcoder Beginner Contest 400)
# 在 Print 之前
已经有近一个月没写文章了，这又是一片 $Atcoder$ 的比赛，场A了4道题，~~第五题没改出来~~。

# A - ABC400 Party
[题目](https://atcoder.jp/contests/abc400/tasks/abc400_a)

虽然是纯英文但大致看懂了，只需判断 $n$ 是否为 $400$ 的倍数即可。

## A - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
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
const ll N=1e1+5;
ll n,ans;
int main() {
	cin>>n;
	ans=400/n;
	if(ans*n!=400) {
		cout<<-1;
	}
	else {
		cout<<ans;
	}
	return 0;
} 
```

# B - Sum of Geometric Series
[题目](https://atcoder.jp/contests/abc400/tasks/abc400_b)

这也没啥好说的，直接模拟即可。

## B - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
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
const ll N=1e1+5;
ll n,ans,k;
int main() {
	cin>>n>>k;
	for(ll i=0;i<=k;i++) {
		ans+=pow(n,i);
		if(ans>1e9) {
			cout<<"inf";
			return 0;
		}
	}
	cout<<ans;
	return 0;
} 
```

# C - 2^a b^2
[题目](https://atcoder.jp/contests/abc400/tasks/abc400_c)

容易发现满足条件的数要么是偶完全平方数，要么是 $2$ 倍的完全平方数。

因此答案就是:

$$
\sum_{a=1}^{\lfloor \log_2 n \rfloor} \left\lfloor \frac{ \left\lfloor \sqrt{\frac{n}{2^a}} \right\rfloor + 1 }{ 2 } \right\rfloor
$$

时间复杂度 $O(logn)$。

p.s. 千万别用 `sqrt`，建议使用二分查找。

## C - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
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
const ll N=1e1+5;
ll ans,n,a=2,num;
int main() {
	cin>>n;
	while(a<=n) {
		ll m=n/a;
		ll l=1,r=m;
		while(l<=r) {
			ll mid=l+((r-l)>>1);
			if(mid<=m/mid) {
				num=mid,l=mid+1;	
			}
			else {
				r=mid-1;
			}
		}
		ans+=(num+1)/2;
		a*=2;
	}
	cout<<ans;
	return 0;
}
```

# D - Takahashi the Wall Breaker
[题目](https://atcoder.jp/contests/abc400/tasks/abc400_d)

考虑转成图论问题求解。

将每个位置 $(x,y)$ 看做节点，向其上下左右 $1∼2$ 步处，共 $8$ 个位置 $(xx,yy)$ 分别建边。

如果 $(x,y)$ 到 $(xx,yy)$ 中途（不包括 $(x,y)$ ）没有经过墙壁，那么边权就是 $0$，否则边权就是 $1$。

最后，从起点到终点的最短路即为答案。

由于边权只有 $0$ 和 $1$ ，我们可以使用 $BFS$ 来求解单源最短路。

时间复杂度 $O(nm)$，其中 $n$ 分别表示点数，$m$ 表示边数。

## D - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
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
	inline void prll(ll x) {
		if(x<0) {
			putchar('-');
			prll(-x);
			return;
		}
		if(x==0) {
			return;
		}
		prll(x/10);
		putchar((char)(x%10+'0'));
	}
}
using namespace io;
const ll N=1e3+5;
ll dx[10]={-1,0,1,0};
ll dy[10]={0,-1,0,1};
ll dis[N][N],n,m,a,b,c,d;
char g[N][N];
deque<pair<ll,ll>>q;
void dfs() {
	memset(dis,0x3f,sizeof(dis));
	dis[a][b]=0;
	q.push_back({a,b});
	while(q.size()) {
		ll x=q.front().first,y=q.front().second;
		q.pop_front();
		for(ll i=0;i<4;i++) {
			ll xx=x+dx[i],yy=y+dy[i];
			if(xx<1||xx>n||yy<1||yy>m||g[xx][yy]=='#'||dis[x][y]>=dis[xx][yy]) {
				continue;
			}
			dis[xx][yy]=dis[x][y];
			q.push_front({xx,yy});
		}
		for(ll i=0;i<4;i++) {
			for(ll j=1;j<=2;j++) {
				ll xx=x+dx[i]*j,yy=y+dy[i]*j;
				if(xx<1||xx>n||yy<1||yy>m||dis[x][y]+1>=dis[xx][yy]) {
					continue;
				}
				dis[xx][yy]=dis[x][y]+1;
				q.push_back({xx,yy});
			}
		}
	}
}
int main() {
	cin>>n>>m;
	for(ll i=1;i<=n;i++) {
		for(ll j=1;j<=m;j++) {
			cin>>g[i][j];
		}
	}
	cin>>a>>b>>c>>d;
	dfs();
	cout<<dis[c][d];
	return 0;
}
```

# E - Ringo's Favorite Numbers 3
[题目](https://atcoder.jp/contests/abc400/tasks/abc400_e)

第五题我代码比较玄幻，再加上没翻译(~~主要是这原因~~)，就直接上代码了。

## E - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
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
	inline void prll(ll x) {
		if(x<0) {
			putchar('-');
			prll(-x);
			return;
		}
		if(x==0) {
			return;
		}
		prll(x/10);
		putchar((char)(x%10+'0'));
	}
}
using namespace io;
const ll N=1e6+5;
ll q;
vector<ll>num(N),ans;
int main() {
	for(ll i=2;i<N;i++) {
		if(num[i]==0) {
			for(ll j=i;j<N;j+=i) {
				num[j]++;
			}
		}
	}
	for(ll i=2;i<N;i++) {
		if(num[i]==2) {
			ans.push_back(i*i);
		}
	}
	cin>>q;
	while(q--) {
		ll x;
		cin>>x;
		cout<<(*prev(upper_bound(ans.begin(),ans.end(),x)))<<endl;
	}
	return 0;
}
```