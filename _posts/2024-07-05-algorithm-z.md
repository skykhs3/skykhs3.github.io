---
title: "Z Algorithm"
date: 2024-07-05 01:29:00 +09:00
categories: [Problem Solving, Algorithm]
post: skykhs3
math: true
tags:
  - z
  - algorithm
  - suffix array
  - ps
  - problem solving
description: Z Algorithm is an algorithm that quickly finds the length of the longest common prefix between each suffix of a string and the entire string in O(N)
---
<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3.2.2/es5/tex-chtml.js">
</script>

<div markdown="1">

## 1. Description

The Z-array for a string $\text{s}$ is an array where $\text{z[i]}$ represents the length of the longest substring starting from the position $\text{i}$ which is also a prefix of $\text{s}$.
The reason the time complexity is $\text{O(n)}$ is due to reusing the results obtained previously.

---

## 2. Example Scenario
Consider the string s = "$\text{ababcabab}$".

The Z-array for this string would be calculated as follows:

### Initialization:

Start with $\text{z[0]}$ = 0 because the entire string $\text{s}$ is trivially a prefix of itself.
For other positions, compute the longest match with the prefix starting from that position.


### Calculating Z-values:

| i   | The substring starting at index i | z[i] |
| --- | --------------------------------- | ---- |
| 0   | $\text{ababcabab}$                | 0    |
| 1   | $\text{babcabab}$                 | 0    |
| 2   | $\text{abcabab}$                  | 2    |
| 3   | $\text{bcabab}$                   | 0    |
| 4   | $\text{cabab}$                    | 0    |
| 5   | $\text{abab}$                     | 4    |
| 6   | $\text{bab}$                      | 0    |
| 7   | $\text{ab}$                       | 2    |
| 8   | $\text{b}$                        | 0    |

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

We will save the maximum of $\text{i+z[i]-1}$ to $\text{r}$, and save $\text{i}$ to $\text{l}$ when $\text{r}$ is updated.

-  Case 1: $\text{i} \leq \text{r}$

    The statement $\text{s[i...r]} = \text{s[(i-l)...(r-l)]}$ is valid

    - Case 1-1: $\text{i} \leq \text{r}$ and $\text{r-i+1} \leq \text{z[i-l]}$

        $\text{r+1}$ is an unexplored index, so we should start exploring from $r+1$ and update $z[i]$

    - Case 1-2: $i\leq r$ and $r-i+1> z[i-l]$
  
        $z[i]=z[i-l]$ is valid

- Case 2: $\text{i}>\text{r}$

    $\text{r+1}$ is an unexplored index, so we should start exploring from $\text{r+1}$ and update $\text{z[i]}$

In Case 1-2, we don't enter the while loop because of the condition. In Cases 1-1 and 2, $\text{i+z[i]}$ will always be greater than the previous $\text{r}$. Thus, $\text{i+z[i]}$ will cover every index form $\text{0}$ to $\text{n-1}$ exactly once. Therefore, the overall time complexity is $\text{O(N)}$.


> [You can test your code here](https://leetcode.com/problems/sum-of-scores-of-built-strings/){:target="_blank"}
{: .prompt-tip }

</div>