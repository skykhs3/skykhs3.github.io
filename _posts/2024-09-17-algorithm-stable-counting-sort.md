---
title: "Stable Counting Sort"
date: 2024-09-17 19:57:00 +09:00
categories: [Problem Solving, Algorithm]
author: skykhs3
math: true
tags:
  - counting sort
  - algorithm
---

<div markdown="1">

## 1. What is a stable sort?

Consider the list [ $a, b, c, d, e, f$ ], where $a = b > c > d > e = f$. <br/> If this list is sorted, the resulting order could be [ $f, e, d, c, b, a$ ], [ $f, e, d, c, a, b$ ], [ $e, f, d, c, b, a$ ], or [ $e, f, c, d, a, b$ ].

If we do consider the original order, the sorting result must preserve the relative order of elements with equal values. For example, if in the original list $e$ comes before $f$, and $a$ before $b$, then a stable sort must place $e$ before $f$, and $a$ before $b$ in the output.

The result of a stable sort is only [ $e, f, c, d, a, b$ ].

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
int getValueOf(const T& elem) {
  // The function `getValueOf()` extracts the key used for sorting.
  // For primitive types like integers, this could simply return the value itself.
  // For user-defined types, it can return an integer attribute used for ordering.
}

template <typename T>
vector<T> stableCountingSort(const vector<T> &a) {
  vector<int> count(MAX_VALUE_RANGE + 1), prefixSum(MAX_VALUE_RANGE + 1);
  vector<T> sorted(a.size());

  // Step 1: Count the occurrences of each element based on their value from getValueOf()
  for (const auto& element : a)
    count[getValueOf(element)]++;
  
  // Step 2: Build the cumulative sum (prefix sum) array
  prefixSum[0] = count[0];
  for (int i = 1; i <= MAX_VALUE_RANGE; i++)
    prefixSum[i] = prefixSum[i - 1] + count[i];

  // Step 3: Place the elements in the correct position in the sorted array
  // This loop goes in reverse to ensure the sort is stable
  for (int i = a.size() - 1; i >= 0; i--)
    sorted[--prefixSum[getValueOf(a[i])]] = a[i];

  return sorted;
}
```

</div>