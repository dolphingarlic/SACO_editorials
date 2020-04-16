---
title: SACO 2019 Booking
date: 2020-04-14 11:25:40
tags:
    - Maths
categories:
    - 2019
    - Round 3
mathjax: true
---

## Solution

<!-- more -->

First, find the factors of $N$.

Next, notice that a solution is possible iff the upperbound of $B$ in the factors is greater or equal to the lowerbound of $A$ in the factors.

We can then simply use a set to find this overlap and the answer will be $\frac{N}{\text{Upperbound of B}}$.

## Complexity

Time: $O(\sqrt{N} + Q  \log{\sqrt{N}})$

Memory: $O(\sqrt{N})$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (ll i = x; i < y; i++)
using namespace std;
typedef long long ll;

deque<ll> factors;
pair<ll, pair<bool, ll>> events[1000001];
ll ans[1000001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    ll n, q;
    cin >> n >> q;
    for (ll i = (ll)sqrt(n); i; i--) if (n % i == 0) {
        factors.push_front(i);
        if (i != (ll)sqrt(n) || (ll)sqrt(n) * (ll)sqrt(n) != n) factors.push_back(n / i);
    }
    while (q--) {
        ll a, b;
        cin >> a >> b;
        ll l = lower_bound(factors.begin(), factors.end(), a) - factors.begin();
        ll r = upper_bound(factors.begin(), factors.end(), b) - factors.begin() - 1;
        if (r < l) cout << "-1\n";
        else cout << n / factors[r] << '\n';
    }
    return 0;
}
```
