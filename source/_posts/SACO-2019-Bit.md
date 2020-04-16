---
title: SACO 2019 Bit
date: 2020-04-14 11:25:30
tags:
    - Square root decomposition
    - Probabilities
categories:
    - 2019
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

We use square root decomposition to solve this problem.

First, keep track of how many blocks are being scanned at the moment. Let this number be $k$.

If $k \leq \sqrt{N}$, simply find the minimum of the $k$ blocks being scanned.

Otherwise, we expect the answer to be around $\sqrt{N}$ or less, since it is highly unlikely that it is greater than that (by the Birthday Paradox).

Because of this, we can simply iterate $i$ from 1 until the block $i$ is being scanned, at which point $i$ is the answer.

## Complexity

Time: $O(M \sqrt{N})$

Memory: $O(N)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

const int rt = 350;
int pos[100001], a[100001], z[100001];
bool scanning[800001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n, m;
    cin >> n >> m;
    FOR(i, 1, n + 1) {
        cin >> a[i];
        pos[a[i]] = i;
    }
    FOR(i, 0, m) {
        cin >> z[i];
        scanning[z[i] + 600000] = true;
    }

    deque<int> q;
    int ptr = m - 1;
    FOR(t, 0, n - z[0] + 1) {
        while (~ptr && z[ptr]+ t > 0) q.push_back(z[ptr--]);
        while (q.size() && q.front() + t > n) q.pop_front();

        if (q.size() <= rt) {
            int mn = INT_MAX, p = -1;
            for (int i : q) if (a[i + t] < mn) mn = a[i + t], p = i + t;
            cout << p << ' ';
        } else {
            FOR(i, 1, n + 1) if (pos[i] - t >= -600000 && scanning[pos[i] - t + 600000]) {
                cout << pos[i] << ' ';
                break;
            }
        }
    }
    return 0;
}
```
