---
title: "Stable Counting Sort"
date: 2024-09-17 19:57:00 +09:00
categories: [Problem Solving, Algorithm]
post: skykhs3
math: true
# image:
#   path: /assets/img/posts/2024-09-11-types-and-options-of-git-merge/--no-ff.png
#   alt: 3-way merge
#   show_in_post: true
tags:
  - counting sort
  - algorithm
---

<div markdown="1">

## 1. What is a stable sort?

Consider the list [ $a, b, c, d, e, f$ ], where $a = b < c < d < e = f$. <br/> If this list is shuffled ( for example, [ $f, e, d, c, b, a$ ] ) and then sorted, the resulting order could be [ $a, b, c, d, e, f$ ], [ $b, a, c, d, e, f$ ], [ $a, b, c, d, f, e$ ], or [ $b, a, c, d, f, e$ ].

If we don't care about the initial shuffled order, all four results are possible outcomes of an unstable sort. However, if we do consider the shuffled order, only the result [ $b, a, c, d, f, e$ ] is valid, since in the shuffled list, $f$ precedes $e$ and $b$ precedes $a$.

**If we don't consider the initial order, it is an unstable sort.**
**If we consider the initial order and maintain the relative order of elements with equal values, it is a stable sort.**

## 2. Counting Sort
Counting Sort is a sorting algorithm that counts the frequency of each value within a specific range and uses that information to sort the array. It is efficient for sorting integers or items with limited range values.

## 3. Code
**Stable Count Sort = Stable Sort + Counting Sort**

### 3.1 Unstable Counting Sort
You would write the code below if you wanted to perform counting sort on integers.

``` c++
vector<int> unstableCountingSort(const vector<int> &a) {
  vector<int> count(MAX_SIZE_OF_VALUE_OF_ELEMENTS + 1);
  vector<int> sorted;
  sorted.reserve(a.size());  // Reserve memory to optimize performance

  // Count occurrences of each element
  for (int element : a)
    count[element]++;

  // Build the sorted array
  for (int i = 0; i <= MAX_SIZE_OF_VALUE_OF_ELEMENTS; i++)
    for (int j = 0; j < count[i]; j++) 
      sorted.push_back(i);
    
  return sorted;
}

```

### 3.2 Stable Counting Sort
We could perform a stable counting sort with **cumulative sum array**.
```c++
template <typename T>
vector<T> stableCountingSort(const vector<T> &a) {
  vector<int> count(MAX_SIZE_OF_ELEMENTS + 1), prefixSum(MAX_SIZE_OF_ELEMENTS + 1);
  vector<T> sorted(a.size());

  // Step 1: Count the occurrences of each element based on their value from getValueOf()
  for (const auto& element : a)
    count[getValueOf(element)]++;
  
  // Step 2: Build the cumulative sum (prefix sum) array
  prefixSum[0] = count[0];
  for (int i = 1; i <= MAX_SIZE_OF_ELEMENTS; i++)
    prefixSum[i] = prefixSum[i - 1] + count[i];

  // Step 3: Place the elements in the correct position in the sorted array
  // This loop goes in reverse to ensure the sort is stable
  for (int i = a.size() - 1; i >= 0; i--)
    sorted[--prefixSum[getValueOf(a[i])]] = a[i];

  return sorted;
}
```

</div>