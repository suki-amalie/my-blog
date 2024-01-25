---
layout: custom_post
title:  "D - Flip Cards"
date:   2024-01-25 12:03:00 +0700
categories: Dynamic Programming
---

### Introduction
[D - Flip Cards](https://atcoder.jp/contests/abc291/tasks/abc291_d)

#### Problem Statement
N cards, numbered 1 through N, are arranged in a line. For each 
i (1≤i<N), card i and card (i+1) are adjacent to each other. Card i has Ai written on its front, and Bi written on its back. Initially, all cards are face up.

Consider flipping zero or more cards chosen from the 
N cards. Among the 2^N ways to choose the cards to flip, find the number, modulo 998244353, of such ways that:

when the chosen cards are flipped, for every pair of adjacent cards, the integers written on their face-up sides are different.

#### Sample Input

```
3
1 2
4 2
3 4
```

#### Sample Output

```
4
```

Let S be the set of card numbers to flip. For example, when 
S={2,3} is chosen, the integers written on their visible sides are 
1,2, and 4, from card 1 to card 3, so it satisfies the condition.

On the other hand, when S={3} is chosen, the integers written on their visible sides are 1,4, and 4, from card 1 to card 3, where the integers on card 2 and card 3 are the same, violating the condition.

Four S satisfy the conditions: {},{1},{2}, and {2,3}.

#### Idea to solution approach

Let's start with N = 1 card then no matter if we flip or not flip the card, the adjacent cards wont share the same value since there's only one card to begin with.

One step forward to N = 2 case, let the 2 cards be:
```
1 2
1 3
```
Since the head of both of the unflipped cards are the same bun their bottoms are different, we can flip anyone of them at once or both of them at once. This gives us 3 S that satisfy the conditions: {1}, {2}, {1, 2}.
```
1 3
2 3
```
S = {},{1}, {2}

```
1 2
3 4
```
Since all the card faces are distinct, no matter which or how many cards we flip, it all satisfy the conditions.
S = {}, {1}, {2}, {1, 2}

N = 3 case:
```
1 2 
1 3 
2 3
```

First let's make sure that the first 2 cards heads are different. Follow up from the N = 2 case, we have 3 different ways of flipping to do this S = {1}, {2}, {1, 2}. Now we only need to flip the 3rd card in such a way that its head will be different with the second card for each of the outcomes:
{1, 3}, {2}, {1, 2}. This suggest a way of calculating incrementally: for the ith card, we look back at the i-1 th card state to determine whether to flip. Now, consider the following DP (Dynamic Programming).

DP[i][flag]= the number of ways to determine whether to flip the first 
i cards such that the conditions are satisfied, where flag represents whether the ith card is flipped or not. In order words the total distinct ways to satisfy the conditions is the number of ways to flip such that the ith card is flipped in the process plus the number of ways to flip where the ith card remained unflipped while still satisfying the conditions.

### Implementation in python
```
MOD = 998244353

N = int(input())
cards = [tuple(map(int, input().split())) for i in range(N)]

#dp consists of tuple of size 2. For each tuple ith, the first index represents the number of ways to reach the state where the ith card is unflipped and the second index represents the number of states where the ith card is flipped.

dp = [[0, 0] for i in range(N)]

# not flipping or flipping one card both satisfy the conditions
dp[0] = [1, 1]

for i in range(1, N):
    for prev in range(2):
        for nxt in range(2):
            if data[i][nxt] != data[i-1][prev]:
                dp[i][nxt] += dp[i-1][prev]
    dp[i][0] %= MOD
    dp[i][1]%= MOD

print((dp[N-1][0] + dp[N-1][1])%MOD)

```





