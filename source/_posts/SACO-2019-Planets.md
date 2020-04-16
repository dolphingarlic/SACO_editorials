---
title: SACO 2019 Planets
date: 2020-04-14 11:26:13
tags:
    - Binary search
    - Maths
    - PIE
categories:
    - 2019
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

Binary search for minimum X where (through the principle of inclusion-exclusion — add the count of all $t_i$ multiples in range, subtract the count of all pairwise LCM of $t_i$ multiples in range, add the count of all triplewise LCM of $t_i$ multiples in range etc.) we find exactly K divisible numbers.

To efficiently find the LCMs, we use bitmask DP and preprocess the LCMs.

## Complexity

Time: $O(2^N  \log{N})$

Memory: $O(2^N)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (ll i = x; i < y; i++)
#pragma GCC Optimize("O3")
#pragma GCC Optimize("unroll-loops")
using namespace std;
typedef long long ll;

ll gcd(ll a, ll b) {
    return (b ? gcd(b, a % b) : a);
}

int n;
ll t[8], dp[1<<8];
bool visited[8];

vector<ll> lcms[8];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    ll k;
    cin >> n >> k;
    FOR(i, 0, n) cin >> t[i];

    dp[0] = 1;
    FOR(i, 1, (1<<n)) {
        int bitcnt = 0;
        for (int j = 0; j < n; j++) if (i & (1 << j)) {
            bitcnt++;
            dp[i] = t[j] * dp[i - (1 << j)] / gcd(t[j], dp[i - (1 << j)]);
        }
        lcms[bitcnt - 1].push_back(dp[i]);
    }

    ll l = 1, r = LLONG_MAX - 1;
    while (l != r) {
        ll mid = (l + r) / 2;
        ll num = 0;
        FOR(i, 0, n) for (ll j : lcms[i]) num += mid / (i & 1 ? -j : j);
        if (num >= k) r = mid;
        else l = mid + 1;
    }

    cout << 2020 + l;
    return 0;
}
```
