# I. Round screws

[Round screws - 题目 - QOJ.ac](https://qoj.ac/contest/2908/problem/15322)

## 思路

如果我要修改一个区间，我可以得到其的最优值取法。

利用 XOR 距离满足三角不等式：$x \bigoplus z \le (x \bigoplus y + y \bigoplus z)$ 

可以的得到区间的一个上界，对于区间 $[l,r]$，不修改 $l,r$，且修改 $[l + 1,r - 1]$，得到该区间的最小贡献为 $a[r] \bigoplus a[l] + (r - l - 1) \times C$ 。

因此我们可以得到一个 $n^2$ 的 $dp$。此时将 $dp$ 进行化简后：$$f_i=\min_{0\le j<i}\left\{f_j+(i-j-1)C+(a_i\oplus a_j)\right\}$$

对异或的值域进行折半，可以将 $n^2$ 的一维优化掉，最终复杂度是 $n\sqrt{V}$ 的。

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
    int n,c;
    std::cin >> n >> c;
    std::vector<int>a(n + 2);
    for(int i = 1;i <= n;i++)std::cin >> a[i];
    std::vector<int>dp(n + 2,INF);
    std::vector g(512,std::vector<int>(512,INF));

    for(int i = 0;i <= n + 1;i++)
    {
        int p = 511 & a[i],q = 511 & (a[i] >> 9);
        for(int j = 0;j < 512;j++)
            dp[i] = std::min(dp[i],c * i + g[j][q] + (j ^ p));
        if(!i)dp[i] = 0;
        int w = dp[i] - (i + 1) * c;
        for(int j = 0;j < 512;j++)
            g[p][j] = std::min(g[p][j],w + 512 * (j ^ q));
    }
    std::cout << dp[n + 1] << "\n";
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

