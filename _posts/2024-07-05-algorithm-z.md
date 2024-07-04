---
title: "Z Algorithm"
date: 2024-07-05 00:01:29 +09:00
categories: [Problem Solving, Algorithm]
tags:
  - z
  - z algorithm
  - string
  - suffix
  - suffix array
  - ps
  - problem solving
---

> The Z-algorithm is an efficient algorithm( O(N) ) that calculates the Z-array for a given string. 

## 1. Overview
The Z-array for a string s is an array z where z[i] represents the length of the longest substring starting from the position i which is also a prefix of s.

## 2. Example Scenario
Consider the string s = "ababcabab". The Z-array for this string would be calculated as follows:

### Initialization:

Start with z[0] = 0 because the entire string s is trivially a prefix of itself.
For other positions, compute the longest match with the prefix starting from that position.


### Calculating Z-values:

For i = 1: No match with the prefix. So, z[1] = 0.

For i = 2: Match "ab" with the prefix "ab". So, z[2] = 2.

For i = 3: No match with the prefix. So, z[3] = 0.

For i = 4: Match "ab" with the prefix "ab". So, z[4] = 2.

For i = 5: Match "abab" with the prefix "abab". So, z[5] = 4.

For i = 6: No match with the prefix. So, z[6] = 0.

For i = 7: Match "ab" with the prefix "ab". So, z[7] = 2.

For i = 8: Match "ab" with the prefix "ab". So, z[8] = 2.

## 3. Code

```c++
class Solution {
public:
    long long sumScores(string s) {
        long long ans=0;
        int n=s.size(),l,r;
        vector<int> z(n);
        l=r=0;
        
        z[0]=n;
        ans+=z[0];

        for(int i=1;i<n;i++){
            if(i<=r) z[i]=min(r-i+1,z[i-l]);
            while(i+z[i]<n && s[z[i]]==s[i+z[i]]) z[i]++;
            if(i+z[i]-1>r){
                l=i;
                r=i+z[i]-1;
            }
            ans+=z[i];
        }

        return ans;
    }
};
```
> [You can test your code here](https://leetcode.com/problems/sum-of-scores-of-built-strings/)
{: .prompt-tip }

