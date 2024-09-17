---
title: "Z Algorithm"
date: 2024-07-05 01:29:00 +09:00
categories: [Problem Solving, Algorithm]
post: skykhs3
tags:
  - z
  - algorithm
  - suffix array
  - ps
  - problem solving
---
<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3.2.2/es5/tex-chtml.js">
</script>

<div markdown="1">

> The Z-algorithm is an efficient algorithm ($$O(N)$$) that calculates the length of the longest substring starting from the each position for a given string. 

## 1. Description

The Z-array for a string $$s$$ is an array where $$z[i]$$ represents the length of the longest substring starting from the position $$i$$ which is also a prefix of $$s$$.
The reason the time complexity is $$O(n)$$ is due to reusing the results obtained previously.

---

## 2. Example Scenario
Consider the string s = "$$ababcabab$$".

The Z-array for this string would be calculated as follows:

### Initialization:

Start with $$z[0]$$ = 0 because the entire string $$s$$ is trivially a prefix of itself.
For other positions, compute the longest match with the prefix starting from that position.


### Calculating Z-values:

|i|The substring starting at index i|z[i]|
|-|-|-|
|0|$$ababcabab$$|0|
|1|$$babcabab$$|0|
|2|$$abcabab$$|2|
|3|$$bcabab$$|0|
|4|$$cabab$$|0|
|5|$$abab$$|4|
|6|$$bab$$|0|
|7|$$ab$$|2|
|8|$$b$$|0|

---

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
            if(i+z[i]-1>r) l=i, r=i+z[i]-1;
            ans+=z[i];
        }
        return ans;
    }
};
``` 
## 4. Proof

We will save the maximum of $$i+z[i]-1$$ to $$r$$, and save $$i$$ to $$l$$ when $$r$$ is updated.

-  Case 1: $$i \leq r$$

    The statement $$s[i...r] = s[(i-l)...(r-l)]$$ is valid

    - Case 1-1: $$i\leq r$$ and $$r-i+1 \leq z[i-l]$$

        $$r+1$$ is an unexplored index, so we should start exploring from $$r+1$$ and update $$z[i]$$

    - Case 1-2: $$i\leq r$$ and $$r-i+1> z[i-l]$$
  
        $$z[i]=z[i-l]$$ is valid

- Case 2: $$i>r$$

    $$r+1$$ is an unexplored index, so we should start exploring from $$r+1$$ and update $$z[i]$$

In Case 1-2, we don't enter the while loop because of the condition. In Cases 1-1 and 2, $$i+z[i]$$ will always be greater than the previous $$r$$. Thus, $$i+z[i]$$ will cover every index form $$0$$ to $$n-1$$ exactly once. Therefore, the overall time complexity is $$O(N)$$.


> [You can test your code here](https://leetcode.com/problems/sum-of-scores-of-built-strings/){:target="_blank"}
{: .prompt-tip }

</div>