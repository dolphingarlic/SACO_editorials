---
title: SACO 2011 Highway Builders
date: 2020-04-14 12:12:02
tags:
    - DSU
    - Data structure
categories:
    - 2011
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

For each road, if you can reach $A$ from node 1 but not $B$, then you can build the highway.

To keep track of connectivity, use DSU.

## Complexity

Time: $O(N + M)$

Memory: $O(N)$

## Code

```cpp
#include <bits/stdc++.h>
#pragma GCC Optimize("O3")
#define FOR(i, x, y) for (int i = x; i < y; i++)
#define MOD 1000000007
typedef long long ll;
using namespace std;

int c[10101];
int find(int a) {
    while (c[a] != a) c[a] = c[c[a]], a = c[a];
    return a;
}
void onion(int a, int b) { c[find(a)] = c[find(b)]; }

int main() {
    iostream::sync_with_stdio(false);
    cin.tie(0);
    int n, m;
    cin >> n >> m;
    FOR(i, 1, n + 1) c[i] = i;
    FOR(i, 0, m) {
        int a, b;
        cin >> a >> b;
        if (find(1) != find(b) && find(a) == find(1)) {
            cout << "YES\n";
            onion(a, b);
        } else cout << "NO\n";
    }
    return 0;
}
```
