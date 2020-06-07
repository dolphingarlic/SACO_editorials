---
title: SACO 2015 Space Jazz
date: 2020-06-07 20:56:31
tags:
    - DP
categories:
    - 2015
    - Round 3
mathjax: true
---

## Solution

<!--more-->

We use dynamic programming to solve this problem.

Let $dp[i][j]$ denote the minimum number of additions to make the range $[i, j]$ "jazzy".

Notice how we will always either match $S_i$ with some $S_k$ where $k \in [i + 1, j]$, or we will insert another $S_i$ at the end of the range $[i, j]$ and match $S_i$ with that.

Therefore, we have

$$dp[i][j] = \min\left(dp[i + 1][j] + 1, \min_{k = i + 1}^j \left(dp[i + 1][k - 1] + dp[k + 1][j] \text{ if } S_i = S_k\right)\right)$$

This recurrence can be implemented in $O(N^3)$ time and gets AC.

## Complexity

Time: $O(N^3)$

Memory: $O(N^2)$

## Code

```cpp
#include <bits/stdc++.h>
#pragma GCC optimize("O3")
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    string s;
    cin >> s;
    int dp[502][502];  // Min additions to get "jazz" from index i to j
                       // Inclusive and 0-indexed
    FOR(i, 0, 502) {
        fill(dp[i], dp[i] + 502, 0);
    }
    FOR(j, 0, s.size() + 1) {
        FOR(i, 0, s.size() - j) {
            dp[i][i + j] = dp[i + 1][i + j] + 1;
            FOR(k, i + 1, i + j + 1) {
                if (s[k] == s[i]) {
                    dp[i][i + j] =
                        min(dp[i][i + j], dp[i + 1][k - 1] + dp[k + 1][i + j]);
                }
            }
        }
    }
    cout << dp[0][s.size() - 1] << '\n';
    return 0;
}
```
