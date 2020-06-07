---
title: SACO 2014 Cunning Plan
date: 2020-06-07 20:50:57
tags:
    - Brute force
    - Data structure
categories:
    - 2014
    - Round 3
mathjax: true
---

## Solution

<!--more-->

Initially, this problem seems like it requires some clever observation because of how large the constraints are.

However, notice that an $O(NQ)$ solution is sufficient: we can simply store the state of the grid and update single points. When we drop a ball, we can just simulate it falling because the path never branches.

The constraints are just low enough for this to pass.

## Complexity

Time: $O(NQ)$

Memory: $O(NM)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

char g[1001][1001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n, m, q;
    cin >> n >> m >> q;
    FOR(i, 1, n + 1) FOR(j, 1, m + 1) cin >> g[i][j];
    while (q--) {
        char c;
        cin >> c;
        if (c == 'U') {
            int x, y;
            char k;
            cin >> k >> x >> y;
            g[x][y] = k;
        } else {
            int x;
            cin >> x;
            FOR(i, 1, n + 1) {
                if (g[i][x] == '/') x--;
                else if (g[i][x] == '\\') x++;
            }
            cout << x << '\n';
        }
    }
    return 0;
}
```
