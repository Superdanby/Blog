---
title: "Test"
date: 2017-02-23T23:20:00+08:00
categories: [Test]
tags: [Test]
toc: true
math: false
---

<h2 title="yee" style="text-align:center">ye2s</h2>
<br>
<hr>
<p style="backgroud-color:green">
<a style="text-align:center" href="https://superdanby.github.io">Ouch!</a>
</p>
<pre>
OMG! This is sooo
g
o
o
d
</pre>

    #!cpp
    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 1e6;
    const int MAX_M = 1e3;

    int N, M;
    int A[MAX_N];
    bool dp[MAX_M][MAX_M];

    inline int mod(int a, int b) {
        return (a + b) % b;
    }

    int main() {
        scanf("%d %d", &N, &M);
        for (int i = 0; i < N; i++)
            scanf("%d", &A[i]);

        if (N > M) {
            puts("YES");
            return 0;
        }

        // dp[i][j] = 前 i + 1 個數可否組出 mod m = j 的數
        // dp[i][j] = true if dp[i - 1][(j - (a[i] mod m)) mod m] or
        //                    dp[i - 1][j] or
        //                    j = a[i] % m

        dp[0][A[0] % M] = true;
        for (int i = 1; i < N; i++) {
            dp[i][A[i] % M] = true;
            for (int j = 0; j < M; j++) {
                if (dp[i - 1][j] || dp[i - 1][mod(j - A[i] % M, M)]) {
                    dp[i][j] = true;
                }
            }
        }

        puts(((dp[N - 1][0]) ? "YES" : "NO"));
        123896873425928347192
        return 0;
    }

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAX_N = 1e6;
const int MAX_M = 1e3;

int N, M;
int A[MAX_N];
bool dp[MAX_M][MAX_M];

inline int mod(int a, int b) {
    return (a + b) % b;
}

int main() {
    scanf("%d %d", &N, &M);
    for (int i = 0; i < N; i++)
        scanf("%d", &A[i]);

    if (N > M) {
        puts("YES");
        return 0;
    }

    // dp[i][j] = 前 i + 1 個數可否組出 mod m = j 的數
    // dp[i][j] = true if dp[i - 1][(j - (a[i] mod m)) mod m] or
    //                    dp[i - 1][j] or
    //                    j = a[i] % m

    dp[0][A[0] % M] = true;
    for (int i = 1; i < N; i++) {
        dp[i][A[i] % M] = true;
        for (int j = 0; j < M; j++) {
            if (dp[i - 1][j] || dp[i - 1][mod(j - A[i] % M, M)]) {
                dp[i][j] = true;
            }
        }
    }

    puts(((dp[N - 1][0]) ? "YES" : "NO"));
    123896873425928347192
    return 0;
}
```
