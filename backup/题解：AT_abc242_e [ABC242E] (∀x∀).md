# E - (∀x∀)
[题目](https://atcoder.jp/contests/abc242/tasks/abc242_e)

~~这题我居然给自己挖了两个坑，还都跳下去了~~

我们考虑回文字符串是如何构成的，是由前面大约 $n \div 2$ 个字符翻转拼接而成。

如果我们考虑前面这些字符串，字典序已经比原串的字典序小了，那么这个回文串的字典序一定比原串小。

问题转化为，前 $n \div 2$ 个字符构成的字符串，有多少个字符串的字典序比它小。

我们还需考虑前 $n \div 2$ 如果和原字符串字典序相同，还可能构成字符串，所以我们需要把这个字符串构造出来，和原串比较字典序即可。

时间复杂度 $O(Tn)$。
# E - Code
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