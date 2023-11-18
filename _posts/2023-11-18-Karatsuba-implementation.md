---
layout: custom_post
title:  "Karatsuba implementation"
date:   2023-11-18 12:03:00 +0700
post_url: 2023-11-18-karatsuba-implementation
---
### Introduction

This is my implementation of Karatsuba recursive multiplication algorithm in C++.

### Implementation

```cpp
#include<iostream>
#include<stdio.h>

using namespace std;

// Helper method: given two unequal sized bit strings, converts them to
// same length by adding leading 0s in the smaller string. Returns the
// the new length

int makeEqualLength(string &str1, string &str2) {
    int len1 = str1.size();
    int len2 = str2.size();
    if (len1 < len2) {
        for (int i = 0; i < len2 - len1; i ++) {
            str1 = '0' + str1;
        }
        return len2;
    } else if (len2 < len1) {
            for (int i = 0; i < len1 - len2; i ++) {
                str2 = '0' + str2;
            }
        }
    return len1;
}

// The main function that adds two bit sequences and returns the addition
string addBitString(string first, string second) {
    string result;

    int length = makeEqualLength(first, second);
    int carry = 0;

    for (int i = length-1; i >=0; i --){
        // convert char into int
        int firstBit = first.at(i) - '0';
        int secondBit = second.at(i) - '0';
        int sum = (firstBit^secondBit^carry) + '0';
        result = (char)sum + result;
        carry = (firstBit&secondBit) | (secondBit&carry) | (firstBit&carry) ;
    }
    // if overflow, then add a leading 1
    if (carry) result = '1' + result;
    return result;
}

// A utility function to multiply single bits of strings a and b
int multiplySingleBit(string a, string b) {
    // convert char to int
    return (a[0] - '0')*(b[0] - '0');
}

// The main function that multiplies two bit strings X and Y and returns
// result as long integer
long int multiply(string a, string b) {
    int n = makeEqualLength(a, b);
    if (n==0) return 0;
    if (n==1) return multiplySingleBit(a, b);

    int firstHalf = n/2; // First half of string, floor(n/2)
    int lastHalf = n - firstHalf; // Second half of string, ceil(n/2)

    string xl = a.substr(0, firstHalf);
    string xr = a.substr(firstHalf, lastHalf);

    string yl = b.substr(0, firstHalf);
    string yr = b.substr(firstHalf, lastHalf);

    long int xlyl = multiply(xl, yl);
    long int xryr = multiply(xr, yr);
    long int xlxrylyr = multiply(addBitString(xl, xr), addBitString(yl, yr));
    return xlyl*(1 << (2*lastHalf)) + (xlxrylyr - xlyl - xryr)*(1 << lastHalf) + xryr;

}

int main() {
    printf ("%ld\n", multiply("1100", "1010"));
    printf ("%ld\n", multiply("110", "1010"));
    printf ("%ld\n", multiply("11", "1010"));
    printf ("%ld\n", multiply("1", "1010"));
    printf ("%ld\n", multiply("0", "1010"));
    printf ("%ld\n", multiply("111", "111"));
    printf ("%ld\n", multiply("11", "11"));
}
```

### Time Complexity
T(n) = 3T(n/2) + O(n) = O(n^2)
