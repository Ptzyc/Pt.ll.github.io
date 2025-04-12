# 在 Print 之前
又是一篇 $Atcoder$ 的题解，星期二(3.4)打了一场 $vp$，赛时做出来了5道，~~第六道赛时觉得太抽象没写出来~~。
# A - T-shirt
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_a)

只需判断 $X \le A$， $A+1 \le X \le B$ ，以及 $B+1 \le X$，这几种情况，输出即可 ~~(没啥好说的)~~。
## A - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e3+5;
ll a,b,c,x;
double ans;
int main() {
    cin>>a>>b>>c>>x;
    if(x<=a) ans=1;
    else if(x>a&&x<=b) {
        ans=c;
        ans/=(b-a);
    }
    printf("%.12lf",ans);
    return 0;
}
```
# B - Minimize Ordering
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_b)

~~更简单~~，只需 $cin$，然后 $sort$，最后 $cout$。
## B - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e5+5;
string s;
int main() {
    cin>>s;
    sort(s.begin(),s.end());
    cout<<s;
    return 0;
}
```
# C - 1111gal password
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_c)

~~有点像数位dp~~。

我们只需要思考上一位比它大一或比它小一的数，把情况加起来即可，即：

$$
num_{i,j}=num_{i-1,j}+num_{i-1,j-1}+num_{i-1,j+1};
$$

我们又会思考，为什么不特判 $1$ 和 $9$ 呢，不难想到， $num_{i,0}$ 和 $num_{i,10}$ 都为 $0$，所以不会影响结果。
## C - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 998244353
using namespace std;
const ll N=1e6+5;
ll n,num[N][15],ans;
int main() {
    cin>>n;
    for(ll i=1;i<=9;i++) {
        num[1][i]=1;
    }
    for(ll i=2;i<=n;i++) {
        for(ll j=1;j<=9;j++) {
            num[i][j]=num[i-1][j]+num[i-1][j-1]+num[i-1][j+1];
            num[i][j]%=mod;
        }
    }
    for(ll i=1;i<=9;i++) {
        ans+=num[n][i];
        ans%=mod;
    }
    cout<<ans;
    return 0;
}
```
# D - ABC Transform
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_d)

我们不难发现，它每次分裂出两个节点，这是啥，二叉树啊，请看图：
![](https://i-blog.csdnimg.cn/img_convert/ee1148d95650e22e9318a384f255ebd1.png)

所以我们只需用一个变量统计一下单复数，左根加 $1$ 右根加 $2$，即：

$$
sum += \begin{cases}
1 & \text{if } \text{ } k \equiv 1 \pmod 2\\
2 & \text{if } \text{ } k \equiv 0 \pmod 2
\end{cases}
$$

注意，如果 $60 \le t$ 那么数肯定大于 $10^{18}$。
## D - Code
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
# E - (∀x∀)
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_e)

~~这题我居然给自己挖了两个坑，还都跳下去了~~

我们考虑回文字符串是如何构成的，是由前面大约 $n \div 2$ 个字符翻转拼接而成。

如果我们考虑前面这些字符串，字典序已经比原串的字典序小了，那么这个回文串的字典序一定比原串小。

问题转化为，前 $n \div 2$ 个字符构成的字符串，有多少个字符串的字典序比它小。

我们还需考虑前 $n \div 2$ 如果和原字符串字典序相同，还可能构成字符串，所以我们需要把这个字符串构造出来，和原串比较字典序即可。

时间复杂度 $O(Tn)$。
## E - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 998244353
using namespace std;
const ll N=1e6+5;
ll T,n,ans,f;
string s,str1,str2;
int main() {
    cin>>T;
    while(T--) {
        ans=0,f=1;
        cin>>n>>s;
        ll num=(n+1)/2;
        for(ll i=0;i<num;i++) {
            ll cnt=s[i]-'A';
            ans*=26;
            ans+=cnt;
            ans%=mod;
        }
        str1="";
        str2="";
        for(ll i=num-1;i>=0;i--) {
            str1+=s[i];
        }
        if(n%2==0) num++;
        for(ll i=num-1;i<n;i++) {
            str2+=s[i];
        }
        if(str1>str2) f=0;
        cout<<(ans+f)%mod<<endl;
    }
    return 0;
}
```
# F - Black and White Rooks
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_f)

题解参照[这篇](https://www.cnblogs.com/maystern/p/15970190.html)。

思路大致相同，但需注意如何 $mod$。
## F - Code
```cpp
#include <bits/stdc++.h>
#define ll long long
#define mod 998244353
using namespace std;
const ll N=5e1+5;
ll n,m,b,w,ans,f[N][N],g[N][N],c[N*N][N*N];
int main() {
	cin>>n>>m>>b>>w;
	c[0][0]=1;
	for(ll i=1;i<=2500;i++) {
		c[i][0]=1;
        c[i][i]=1;
		for(ll j=1;j<=2500;j++) {
			c[i][j]=(c[i-1][j-1]+c[i-1][j])%mod;
		}
	}
	for(ll i=1;i<=n;i++) {
		for(ll j=1;j<=m;j++) {
			f[i][j]=c[i*j][b];
			for(ll l=1;l<=i;l++) {
				for(ll r=1;r<=j;r++) {
					if(l==i&&r==j) continue;
                    ll cnt=f[l][r];
                    cnt*=c[i][l];
                    cnt%=mod;
                    cnt*=c[j][r];
                    cnt%=mod;
					f[i][j]=(f[i][j]-(cnt+mod)%mod+mod)%mod;
				}
            }
		}
	}
	for(ll i=1;i<=n;i++) {
		for(ll j=1;j<=m;j++) {
			g[i][j]=c[i*j][w];
			for(ll l=1;l<=i;l++) {
				for(ll r=1;r<=j;r++) {
					if(l==i&&r==j) continue;
                    ll cnt=g[l][r];
                    cnt*=c[i][l];
                    cnt%=mod;
                    cnt*=c[j][r];
                    cnt%=mod;
					g[i][j]=(g[i][j]-(cnt+mod)%mod+mod)%mod;
				}
            }
		}
	}
	for(ll i=1;i<=n;i++) {
		for(ll j=1;j<=n-i;j++) {
			for(ll l=1;l<=m;l++) {
				for(ll r=1;r<=m-l;r++) {
                    ll cnt=c[n][i];
                    cnt*=c[n-i][j];
                    cnt%=mod;
                    cnt*=c[m][l];
                    cnt%=mod;
                    cnt*=c[m-l][r];
                    cnt%=mod;
                    cnt*=f[i][l];
                    cnt%=mod;
                    cnt*=g[j][r];
                    cnt%=mod;
					ans+=cnt;
                    ans%=mod;
				}
			}
		}
	}
	cout<<ans;
	return 0;
}
```