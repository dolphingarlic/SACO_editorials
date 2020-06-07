---
title: SACO 2017 Towers
date: 2020-04-14 12:13:45
tags:
    - 2 pointers
categories:
    - 2017
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

Firstly, notice that we only care about the closest tower to each building. The answer is simply the maximum of the distances from each building to their closest tower.

The closest tower can either be on the left or the right of the building. We can simply use the STL's `upper_bound` to find the 2 towers on either side of the building.

We can use 2 pointers to bring the complexity down to $O(N)$ but this isn't necessary.

## Complexity

Time: $O(N log N)$

Memory: $O(N)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int a[1000000], b[1000000];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n, m;
    cin >> n >> m;
    FOR(i, 0, n) cin >> a[i];
    FOR(i, 0, m) cin >> b[i];

    int ans = 0;
    FOR(i, 0, n) {
        int ptr = upper_bound(b, b + m, a[i]) - b;
        int dist = INT_MAX;
        if (ptr < m) dist = min(dist, b[ptr] - a[i]);
        if (ptr) dist = min(dist, a[i] - b[ptr - 1]);
        ans = max(ans, dist);
    }

    cout << ans;
    return 0;
}
```