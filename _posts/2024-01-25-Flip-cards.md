---
layout: custom_post
title:  "Karatsuba implementation"
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

