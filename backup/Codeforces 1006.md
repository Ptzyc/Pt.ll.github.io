# 在 Print 之前
鄙人早上考了一场 $VP$,但比赛时只拿下了5道题，大概排在全球 $4624$ 位
# A - New World, New Me, New Array
[题目](https://codeforces.com/problemset/problem/2072/A)

也是非常感人的一题，有点贪心的影子，最后模拟一下即可。

每一个数都尽可能改成 $p$，符号与 $k$ 相同即可。如果全部元素都改完还不行即 $np<\left | k \right |$ 就输出 $-1$ 。
## A - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e1+5;
ll T,n,k,p;
int main() {
    cin>>T;
    while(T--) {
        cin>>n>>k>>p;
        if(k<0) k=-k;
        if(n*p<k) {
            cout<<-1<<endl;
        }
        else {
            ll num=k/p;
            if(num*p==k) {
                cout<<num;
            }
            else {
                cout<<num+1;
            }
            cout<<endl;
        }
    }
    return 0;
}
```
# B - Having Been a Treasurer in the Past, I Help Goblins Deceive
[题目](https://codeforces.com/problemset/problem/2072/B)

p.s. ~~这么简单的题我居然用了40多min~~

本题只需统计一下 `_` 和 `-` 的个数即可，最后用 `_` 的个数乘上 `-` 的个数除以二即可。
## B - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e1+5;
ll T,n,ans,num,sum;
string s;
bool a[N];
ll C(ll x) {
    return x;
}
int main() {
    cin>>T;
    while(T--) {
        num=0;
        sum=0;
        cin>>n>>s;
        for(ll i=0;i<n;i++) {
            if(s[i]=='_') num++;
            else sum++;
        }
        ans=0;
        ans+=C(sum/2)*C((sum+1)/2)*num;
        cout<<ans<<endl;
    }
    return 0;
}
```
# C - Creating Keys for StORages Has Become My Main Skill
[题目](https://codeforces.com/problemset/problem/2072/C)

本题很好想，只需思考考虑 $MEX$ 的特性我们只需要从 $i=0$ 开始往数组中填数，每次保证 $x|i==x$。

因为或运算可以交换顺序，数组中所有元素或运算之后结果为 $x$ 的必要条件为 $x|i==x(\forall i\subset a)$。

如果一旦不能填数了那么就终止循环，后面全填 $x$ 即可因为 $x|x==x$。
## C - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e1+5;
ll T,n,a[N],x;
int main() {
	cin>>T;
	while(T--) {
		cin>>n>>x;
		ll num=0,cnt=0,lt=0,i=0;
		for(i=0;i<=2*x&&cnt<n-1;i++) {
			if((x|i)==x) {
				cnt++;
				cout<<i<<' ';
				num=num|i;
				if(i>1&&(i-lt)!=1) break;
				lt=i;
			} else {
				if(i>1&&(i-lt)!=1) break;
				continue;
			}
		}
		while(cnt!=n) {
			cnt++;
			if((num|i)==x) cout<<i<<' ';
			else cout<<x<<' ';
		}
		cout<<endl;
	}
	return 0;
}
```
# D - For Wizards, the Exam Is Easy, but I Couldn't Handle It
[题目](https://codeforces.com/problemset/problem/2072/D)

~~这题是一眼的线段树~~

毕竟是求逆序对的个数以及位置，并且题目都说了：

>保证所有测试用例的 $n^2$ 之和不超过 $4 \cdot 10^6$

所以用 $cnt$ 记录答案的优劣程度即逆序对减少的个数，越大越优。

两次循环即可，分别枚举左边界与右边界，不断维护前缀和 $cnt$。

~~当然你也可以用线段树来优化~~
## D - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e3+5;
ll T,n,a[N];
int main() {
	cin>>T;
	while(T--) {
		cin>>n;
		for(ll i=1;i<=n;i++) {
			cin>>a[i];
		}
		ll l=1,r=1,mn=0;
		for(ll l1=1;l1<=n;l1++) {
			ll cnt=0;
			for(ll r1=l1+1;r1<=n;r1++) {
				if(a[r1]>a[l1]) cnt++;
				if(a[r1]<a[l1]) cnt--;
				if (cnt<mn) {
					mn=cnt;
					l=l1;
					r=r1;
				}
			}
		}
		cout<<l<<' '<<r<<endl;
	}
	return 0;
}
```
# E - Do You Love Your Hero and His Two-Hit Multi-Target Attacks?
[题目](https://codeforces.com/problemset/problem/2072/E)

只有两个点在同一行或者同一列，才满足 $|a_{x}-b_{x}|+|a_{y}-b_{y}|=\sqrt{(a_{x}-b_{x})^{2}+(a_{y}-b_{y})^{2}}$ 的条件。

一次尽可能多的在同一行放点，如在同一行放了 $m$ 个点，则会为答案贡献 $C\binom{n}{2}$。

不断模拟尽可能在同一行放即可，放不了去下一行放。

需要注意的是不要使这些点在同一列，否则也会增加贡献而且会变得很难想了。
## E - Code
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=2e3+5;
ll T,k;
ll C(ll n) {
	return (n*(n-1))>>1;
}
vector<ll> v;
int main() {
	cin>>T;
	while(T--) {
		cin>>k;
        v.clear();
		ll num=k;
		while(num>0) {
			ll n=1;
			while(C(n+1)<=num) {
				n++;
            }
			v.push_back(n);
			num-=C(n);
		}
        ll cnt=0,ans;
        for(ll i=0;i<v.size();i++) {
            cnt+=v[i];
        }
		cout<<cnt<<endl;
		for(ll i=0;i<v.size();i++) {
			for(ll j=0;j<v[i];j++) {
				cout<<ans<<" "<<i<<endl;
				ans++;
			}
		}
	}
	return 0;
}
```
# F - Goodbye, Banker Life
[题目](https://codeforces.com/problemset/problem/2072/F)

此题很像杨辉三角，但题目中说：
![F-1](https://i-blog.csdnimg.cn/direct/12f22c96680040dfa2e8784fba90babb.png#pic_center)

感谢[W_Sherlock_Henry](https://blog.csdn.net/W_Sherlock_Henry)提供的图片。

所以很容易想到分治，但是用分治过于麻烦，所以我们可以用到亿点点数论：

不难发现，数组里的数字要么是 $x$ 要么是 $0$，想想就能知道，对于中间的数，如果进行了偶数次和 $x$ 异或，那就是 $0$，如果进行了奇数次和 $x$ 异或计算，那就是 $x$。

那我们就可以转到杨辉三角形，杨辉三角形还有一个很重要的性质就是组合数，由于n的范围是1e6，如果直接计算就会爆 ，我们就要考虑优化的方法.

众所周知：

$$
C_{n}^{i}=C_{n}^{i-1}*(n-i+1)/i
$$

那么我们只要知道分子总共有多少个2，分母有多少个2，就能知道当前数字是偶数还是奇数。

## F - Code
```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const ll N=1e6+5;
ll a[N],num[N],T,n,x,cnt=1;
void init() {
	for(ll i=2;i<=500&&num[i-1]<=100000;i++) {
		num[i]=num[i-1]+cnt;
		cnt++;
	}
}
int main() {
	init();
	cin>>T;
	while(T--) {
		cin>>n>>x;
		if(n==1) {
			cout<<x<<endl;
			continue;
		}
		ll sum=0,ans=0,num;
		cout<<x<<' ';
		for(ll i=1;i<n;i++) {
			num=n-i;
			while(num%2==0) {
				num/=2;
				sum++;
			}
			num=i;
			while(num%2==0) {
				num/=2;
				ans++;
			}
			if(sum>ans) cout<<0<<' ';
			else cout<<x<<' ';
		}
		cout<<endl;
	}
	return 0;
}
```
# 后记
希望下次能有好成绩。