[题目传送门](https://www.luogu.com.cn/problem/P7054)
# **题目分析**
题目说的很清楚，如果没有 $k$ 就是纯**拓扑**；

题目说最小的拓扑序的最大，俗称第一位最小，第二位第二小等；这一看就是**贪心**；

虽说知道了大致方向但如何贪又是一个问题；
# **思路**
贪心：对于当前入度为 $0$ 的点，假如有多个，给比较小的点加一个连向它的边，然后选大一点的点。

拓扑：在拓扑中不断进行选择，从 $in$ 为 $0$ 的点开始依次抉择，每一步找一个小一点的点来连；

所以总体思路就是贪心和拓扑；

那么怎么存呢？

众所周知，实现拓扑时会使用到**队列**这~~很容易~~想到 `priority_queue` 来排最小字典序的拓扑序，因为为了满足字典序最小，我把拓扑排序的队列换成小根堆，并且记录路径（其实无所谓，用 `priority_queue` 也行）；

我们要让连边后的拓扑序字典序最大，那就要让越靠前的数越大，不难想到：我们只需要用一个大根堆来存储当前能与其他点连边的点就行了（大根堆会自动从大到小排序）。
# **Code**

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e5+5;
ll n,in[N],m,k;
struct node {
    ll u,v;
};
vector<ll> v[N];
vector<node> ans;
priority_queue<ll,vector<ll>,greater<ll>> pq;
priority_queue<ll,vector<ll>,less<ll>> q;//用priority_queue<ll>也行
void tosort() {
    ll root=-1;
    while(!q.empty()||!pq.empty()) {
        while(!pq.empty()&&k>0) {
            ll num=pq.top();
            pq.pop();
            ll sum=q.empty()?0:q.top();
            if(sum<num&&pq.empty()) {
                pq.push(num);
                break;
            }
            q.push(num);
            k--;
        }
        ll num=-1;
        if(pq.empty()) {
            num=q.top();
            q.pop();
            if(root!=-1) {
                ans.push_back({root,num});
            }
        }
        else {
            num=pq.top();
            pq.pop();
        }
        cout<<num<<' ';
        for(ll i=0;i<v[num].size();i++) {
            in[v[num][i]]--;
            if(in[v[num][i]]==0) {
                pq.push(v[num][i]);
            }
        }
        root=num;
    }
}
int main() {
    cin>>n>>m>>k;
    for(ll i=1;i<=m;i++) {
        ll x,y;
        cin>>x>>y;
        v[x].push_back(y);
        in[y]++;
    }
    for(ll i=1;i<=n;i++) {
        if(in[i]==0) {
            pq.push(i);
        }
    }
    tosort();
    cout<<endl<<ans.size()<<endl;
    for(ll i=0;i<ans.size();i++) {
        cout<<ans[i].u<<' '<<ans[i].v<<endl;
    }
    return 0;
}
```
感谢 [plh](https://www.luogu.com.cn/user/427045) 对本题解的帮助。