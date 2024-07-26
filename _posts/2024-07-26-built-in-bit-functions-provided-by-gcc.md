---
title: "Built-in Bit Functions Provided By GCC"
date: 2024-07-26 00:03:13 +09:00
categories: [Development, C/C++]
tags:
  - built-in
  - c
  - c++
  - bits
  - lcp
  - ps
  - problem solving
  - __builtin_clz(x)
  - __builtin_ctz(x)
  - __builtin_popcount
  - __builtin_parity
  - __builtin_clzll
  - __builtin_ctzll
  - __builtin_popcountll
  - __builtin_parityll
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<div markdown="1">
> GCC includes built-in bit functions. These functions can perform tasks such as counting the number of 1-bits in a number, and their time complexity is O(1).

>[6.63 Other Built-in Functions Provided by GCC](https://gcc.gnu.org/onlinedocs/gcc/Other-Builtins.html){:target="_blank"}
{: .prompt-info }


## 1. Overview
In theory, counting the number of 1-bits in a number 𝑁 has a time complexity of 𝑂(log𝑁) since it involves repeatedly dividing the number by two until it becomes zero. However, using built-in functions, this task can be accomplished in 𝑂(1) time complexity.

## 2. Instructions

### Code
For long long type literals, you need to append "ll" to the functions.
``` c++
#include <iostream>

int main(){
  // 29 = 00000000 00000000 00000000 00011101 in binary
  unsigned int x = 29; 

  // Count leading zeros => 27
  std::cout << "__builtin_clz(x) = " << __builtin_clz(x) << endl;

 // Count trailing zeros => 0
  std::cout << "__builtin_ctz(x) = " << __builtin_ctz(x) << endl;

 // Count number of 1 bits => 4
  std::cout << "__builtin_popcount(x) = " << __builtin_popcount(x) << endl;

 // Parity (1 if the number of 1 bits is odd, 0 if even) => 0
  std::cout << "__builtin_parity(x) = " << __builtin_parity(x) << endl;

  // 12345678987654321 = 00000000 00100011 01001011 11100010 01010001 10001010 10001110 11110101
  unsigned long long y = 12345678987654321ULL;

// Count leading zeros for long long => 10
  std::cout << "__builtin_clzll(y) = " << __builtin_clzll(y) << endl; 

 // Count trailing zeros for long long => 0
  std::cout << "__builtin_ctzll(y) = " << __builtin_ctzll(y) << endl;

 // Count number of 1 bits for long long => 27
  std::cout << "__builtin_popcountll(y) = " << __builtin_popcountll(y) << endl;

 // Parity for long long => 1
  std::cout << "__builtin_parityll(y) = " << __builtin_parityll(y) << endl;

  return 0;
}
```

### Output
```
__builtin_clz(x) = 27
__builtin_ctz(x) = 0
__builtin_popcount(x) = 4
__builtin_parity(x) = 0
__builtin_clzll(y) = 10
__builtin_ctzll(y) = 0
__builtin_popcountll(y) = 27
__builtin_parityll(y) = 1
```
## Custom functions vs Built-in function
Comparison of **execution time** between the custom function and the built-in function.

### Code
``` c++
#include <time.h>
#include <stdio.h>

const int LIMIT = 100000000;

// Function to count the number of 1 bits using a loop
int getPopCountLoop(int num) {
    int count = 0;
    while (num) {
        if (num & 1) count++;
        num >>= 1;
    }
    return count;
}

// Function to count the number of 1 bits without using a loop
int getPopCountNotLoop(int num) {
    return (num >> 30 & 1)
         + (num >> 29 & 1)
         + (num >> 28 & 1)
         + (num >> 27 & 1)
         + (num >> 26 & 1)
         + (num >> 25 & 1)
         + (num >> 24 & 1)
         + (num >> 23 & 1)
         + (num >> 22 & 1)
         + (num >> 21 & 1)
         + (num >> 20 & 1)
         + (num >> 19 & 1)
         + (num >> 18 & 1)
         + (num >> 17 & 1)
         + (num >> 16 & 1)
         + (num >> 15 & 1)
         + (num >> 14 & 1)
         + (num >> 13 & 1)
         + (num >> 12 & 1)
         + (num >> 11 & 1)
         + (num >> 10 & 1)
         + (num >> 9 & 1)
         + (num >> 8 & 1)
         + (num >> 7 & 1)
         + (num >> 6 & 1)
         + (num >> 5 & 1)
         + (num >> 4 & 1)
         + (num >> 3 & 1)
         + (num >> 2 & 1)
         + (num >> 1 & 1)
         + (num >> 0 & 1);
}

int main() {
    int count1 = 0, count2 = 0, count3 = 0;
    clock_t start, finish;
    double duration;

    // Measure time using loop-based function
    start = clock();
    for (int i = 1; i <= LIMIT; i++) {
        count1 += getPopCountLoop(i);
    }
    finish = clock();
    duration = (double)(finish - start) / CLOCKS_PER_SEC;
    printf("%lf s <- Using Loop\n", duration);

    // Measure time using non-loop function
    start = clock();
    for (int i = 1; i <= LIMIT; i++) {
        count2 += getPopCountNotLoop(i);
    }
    finish = clock();
    duration = (double)(finish - start) / CLOCKS_PER_SEC;
    printf("%lf s <- Not Using Loop\n", duration);

    // Measure time using built-in function
    start = clock();
    for (int i = 1; i <= LIMIT; i++) {
        count3 += __builtin_popcount(i);
    }
    finish = clock();
    duration = (double)(finish - start) / CLOCKS_PER_SEC;
    printf("%lf s <- Built-in function\n", duration);

    // Print results
    printf("\n%d %d %d\n", count1, count2, count3);

    return 0;
}

```

### Output

Time: Using Loop > Not Using Loop > Built-in function 

```
7.639704 s <- Using Loop
0.647321 s <- Not Using Loop
0.067076 s <- Built-in function

1314447116 1314447116 1314447116
```

## 4. How these functions can perform in O(1) time complexity?
### 1. Hardware Support

Modern CPUs provide special instructions for bit manipulation operations, which are executed in constant time. 

Examples include:

- **CLZ (Count Leading Zeros)**: Instructions like **LZCNT** on x86 or **CLZ** on ARM.
- **CTZ (Count Trailing Zeros)**: Instructions like **TZCNT** on x86 or **RBIT combined with CLZ** on ARM.
- **POPCOUNT (Population Count)**: Instructions like **POPCNT** on x86 or **VCNT** on ARM.
- **PARITY**: Instructions like **POPCNT** can be used to determine parity by checking the number of 1s.

### 2. Compiler Optimization

The GCC and Clang compilers are optimized to translate these built-in functions directly into the corresponding hardware instructions. This means that when you use a built-in function like $$\text{__builtin_clz}$$, the compiler generates code that directly utilizes the efficient hardware instructions.

### 3. Constant Time Execution

Since these hardware instructions are executed in a fixed number of cycles, the operations they perform (counting leading zeros, counting trailing zeros, counting the number of 1 bits, or determining parity) are completed in constant time, $$O(1)$$. The complexity does not depend on the size of the input; instead, it is bounded by the nature of the instruction set of the CPU.

## 4. Applications

You could find the highest power of two that is smaller than or equal to 𝑛 in 𝑂(1) time complexity.
```c++
int highestPowerOf2(int n) {
    if (n <= 0) return 0; // There is an assumption that n is positive, but additional safeguards
    return 1 << (31 - __builtin_clz(n));
}

long long highestPowerOf2(long long n) {
    if (n <= 0) return 0; // There is an assumption that n is positive, but additional safeguards
    return 1LL << (63 - __builtin_clzll(n));
}
```

</div>
