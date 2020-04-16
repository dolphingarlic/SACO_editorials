---
title: SACO 2018 Buttons
date: 2020-04-14 11:50:57
tags:
    - Maths
categories:
    - 2018
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

Consider the problem in reverse:

Instead of decreasing and multiplying $N$ by $A$, increase and divide $M$ by $A$.

Since $N$ and $M$ must be integers at all points, this limits when we may divide by $A$.

Simply keep increasing $M$ and then divide by $A$ as soon as $A$ divides $M$, which is optimal.

## Complexity

Time: $O(\log_A{M})$

Memory: $O(1)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for(int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    ll a, n, m, cnt = 0;
    cin >> a >> n >> m;

    while (m > n) {
        if (m % a == 0) {
            m /= a;
        } else {
            m++;
        }
        cnt++;
    }
    return cout << cnt + (n - m) << '\n', 0;
}
```
