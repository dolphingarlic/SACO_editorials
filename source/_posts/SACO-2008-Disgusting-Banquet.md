---
title: SACO 2008 Disgusting Banquet
date: 2020-06-07 20:26:23
tags:
    - DP
    - Aliens trick
    - Binary search
categories:
    - 2008
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

The optimal solution uses dynamic programming. Instead of finding the maximum tastiness we can eat, find the maximum repulsiveness we can avoid and subtract that from the sum of all $T_i$.

If we let $dp[i][j]$ denote the solution for the first $i$ elements of the given array and $j$ subarrays to be taken, then we have

$$dp[i][j] = \max\left(dp[i - 1][j], \max_{k = 1}^{i - 1}\left(dp[k - 1][j - 1] - \sum_{l = k}^i T_l \right)\right)$$

This recurrence can be computed in $O(N^2)$ time if implemented carefully, which gets us about 70 points.

If we have no restriction on the number of subarrays we take (i.e. $K = \infty$), then we will take every subarray with only positive values.

The trick here now is to assign a cost to each subarray we take so that we don't take too many of them. Notice how if we increase the cost, we never take more subarrays than we would have if the cost was lower.

This means we can binary search for the cost until we take at most $K$ subarrrays!

If we let $dp_\lambda[i]$ denote the solution for the first $i$ elements of the given array, where adding a subarray comes with cost $\lambda$, then we have

$$dp_\lambda[i] = \max\left(dp_\lambda[i - 1], \max_{j = 1}^{n - 1}\left(dp_\lambda[j - 1] - \lambda - \sum_{k = j}^i T_k \right)\right)$$

See [this great tutorial](http://serbanology.com/show_article.php?art=The%20Trick%20From%20Aliens) for more details. Problems that use this trick include [IOI 2016 Aliens](https://oj.uz/problem/view/IOI16_aliens) and [NOI.sg 2019 Feast](https://oj.uz/problem/view/NOI19_feast) (basically the same problem)

## Complexity

Time: $O(N \log N)$

Memory: $O(N)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

ll a[50001], dp[50001];
int cnt[50001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    ll n, k, sm = 0;
    cin >> n >> k;
    FOR(i, 1, n + 1) {
        cin >> a[i];
        sm += a[i];
        a[i] *= -1;
    }

    ll l = 0, r = 3e14, ans = 0;
    while (l != r) {
        ll mid = (l + r) / 2;

        ll local_max = 0, local_max_cnt = 0;
        FOR(i, 1, n + 1) {
            local_max += a[i];
            if (dp[i - 1] >= local_max - mid) {
                dp[i] = dp[i - 1];
                cnt[i] = cnt[i - 1];
            } else {
                dp[i] = local_max - mid;
                cnt[i] = local_max_cnt + 1;
            }

            if (local_max < dp[i - 1]) {
                local_max = dp[i - 1];
                local_max_cnt = cnt[i - 1];
            }
        }

        if (cnt[n] <= k) {
            r = mid;
            ans = max(ans, dp[n] + mid * cnt[n]);
        } else
            l = mid + 1;
    }

    cout << sm + ans;
    return 0;
}```
