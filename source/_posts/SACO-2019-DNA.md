---
title: SACO 2019 DNA
date: 2020-04-14 11:26:02
tags:
    - DP
    - Trie
    - Strings
categories:
    - 2019
    - Round 3
mathjax: true
---

## Solution

This is a DP problem.

Let $dp[i]$ be the minimum number of DNA strings we use to construct the word up to position $i$.

$dp[i] = 2^{60}$ if it is not possible to make the word up to position $i$.

The recurrence is as follows:

$$dp[i] = \min_{k < i}(dp[k] + 1 \text{ } \vert \text{ } S[k:i] \text{ is a DNA string})$$

Since $S$ is fixed, we can simply use a trie to find the valid $k$ for each $i$.

## Complexity

Time: $O(N^2 + M)$

Memory: $O(N + M)$

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (ll i = x; i < y; i++)
using namespace std;
typedef long long ll;

map<char, int> val = {{'A', 0}, {'T', 1}, {'C', 2}, {'G', 3}};

struct TrieNode {
    TrieNode *children[4];
    bool is_end;

    TrieNode() {
        FOR(i, 0, 4) children[i] = nullptr;
        is_end = false;
    }
};

void Insert(TrieNode *node, string s) {
    TrieNode *pCrawl = node;
    for (char i : s) {
        if (!pCrawl->children[val[i]]) pCrawl->children[val[i]] = new TrieNode();
        pCrawl = pCrawl->children[val[i]];
    }
    pCrawl->is_end = true;
}

ll dp[5001], l[100001];
string k[100001];

TrieNode *root;

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    root = new TrieNode();
    int n, m;
    string s;
    cin >> n >> m >> s;
    FOR(i, 0, m) {
        cin >> l[i] >> k[i];
        reverse(k[i].begin(), k[i].end());
        Insert(root, k[i]);
    }

    dp[0] = 0;
    FOR(i, 1, n + 1) {
        dp[i] = (1ll << 60);
        TrieNode *pCrawl = root;
        for (int j = i - 1; ~j; j--) {
            if (!pCrawl->children[val[s[j]]]) break;
            else pCrawl = pCrawl->children[val[s[j]]];

            if (pCrawl->is_end) dp[i] = min(dp[i], dp[j] + 1);
        }
    }

    if (dp[n] == (1ll << 60)) cout << -1 << '\n';
    else cout << dp[n] << '\n';
    return 0;
}
```
