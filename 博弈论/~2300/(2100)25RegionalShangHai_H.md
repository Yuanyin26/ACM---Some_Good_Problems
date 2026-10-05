# H. AGI

[AGI - 题目 - QOJ.ac](https://qoj.ac/contest/2908/problem/15321/statement/zh_cn)

## 思路

将相同的数字两两配对 $(x,x)$

现在剩余一些无法配对且互不相同的数，数量为 $len$。

对于每一个配对，$Menji$ 或者 $Bot$ 都可以强制做到一人一个。那此时有固定的异或和 $X$ 。

当 $len == 0$ 时，$X$ 如果为 $0$，$Menji$ 获胜，否则 $Bot$ 获胜；

当 $len == 2$ 时，如果选择其中一个 $u$ 使得 $X$ ^ $u == 0$，此时 $Menji$ 获胜，否则 $Bot$ 获胜；

当 $len >= 4$ 时，如果 $Menji$ 选择 $u$ ，对于剩余的部分，有且只有一个 $v$ 使得 $X$ ^ $u$ ^ $v == 0$ ，此时 $Bot$ 删掉这个 $v$ 即可。后续是一个递归的过程，此时 $Menji$ 不可能获胜。 

## 代码

```c++
#include<bits/stdc++.h>
#define IOS std::ios::sync_with_stdio(0);std::cin.tie(0);std::cout.tie(0);
#define inf 0x3f3f3f3f
#define INF 4557430888798830399
#define int long long
using i64 = long long;
using ull = unsigned long long;
using i128 = __int128_t;
using pii = std::pair<int,int>;
using ull = unsigned long long;
using ld = long double;
constexpr int N = 2e5 + 7;
constexpr int P = 1e9 + 7;
constexpr double eps = 1e-9;

void solve()
{
    int n;std::cin >> n;n <<= 1;
    std::vector<int>a(n + 1),tmp;
    for(int i = 1;i <= n;i++)
    {
        std::cin >> a[i];
        tmp.push_back(a[i]);
    }
    std::sort(tmp.begin(),tmp.end());
    tmp.erase(std::unique(tmp.begin(),tmp.end()),tmp.end());
    int len = tmp.size();
    std::vector<int>cnt(len);
    for(int i = 1;i <= n;i++)
        cnt[std::lower_bound(tmp.begin(),tmp.end(),a[i]) - tmp.begin()]++;
    std::vector<int>odd;
    int now = 0;
    for(int i = 0;i < len;i++)
    {
        if(cnt[i] & 1)odd.push_back(tmp[i]);
        cnt[i] >>= 1;
        if(cnt[i] & 1)now ^= tmp[i];
    }
    if(odd.empty() && !now)std::cout << "Menji\n";
    else if(odd.size() == 2 && (!(now ^ odd[0]) || !(now ^ odd[1])))std::cout << "Menji\n";
    else std::cout << "Bot\n";

}
signed main()
{
    // init();
    IOS;
    int T = 1;std::cin >> T;
    while(T--)
    {
        solve();
    }
    return 0;
}
```

