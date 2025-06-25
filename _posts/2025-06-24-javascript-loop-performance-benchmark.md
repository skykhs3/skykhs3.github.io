---
title: "JavaScript: Loop Performance Benchmark"
date: 2025-06-24 14:58:00 +09:00
categories: [Development,  JavaScript/TypeScript]
author: skykhs3
image:
  path: /assets/img/posts/javascript-logo.webp
  alt: JavaScript Loop Performance Benchmark
  show_in_post: false
tags:
  - javascript
description:  I wondered if array methods are slower than the traditional for loop.

---

I wondered which loop algorithm **(for(let i = 0; i < arr.length; i++), (forEach), (for...of), (for...in), (Object.keys()), (Object.entries()))** is the fastest.

So I created a benchmark.

## 1. PerformanceMeasurer
```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.PerformanceMeasurer = void 0;
// Performance measurement utilities
class PerformanceMeasurer {
  constructor() {
    this.stats = {};
  }
  measure(name, fn, iterations = 1) {
    if (!this.stats[name]) {
      this.stats[name] = { times: [], total: 0, min: Infinity, max: 0 };
    }
    for (let i = 0; i < iterations; i++) {
      const start = performance.now();
      fn();
      const end = performance.now();
      const time = end - start;
      this.stats[name].times.push(time);
      this.stats[name].total += time;
      this.stats[name].min = Math.min(this.stats[name].min, time);
      this.stats[name].max = Math.max(this.stats[name].max, time);
    }
  }
  getStats(name) {
    return this.stats[name];
  }
  getAllStats() {
    return this.stats;
  }
  calculateMedian(times) {
    const sorted = [...times].sort((a, b) => a - b);
    const mid = Math.floor(sorted.length / 2);
    return sorted.length % 2 === 0
      ? (sorted[mid - 1] + sorted[mid]) / 2
      : sorted[mid];
  }
  printSummary(title, iterations, dataSize) {
    console.log(`\n=== ${title} ===`);
    console.log(`Total iterations: ${iterations}`);
    console.log(`Data size: ${dataSize}\n`);
    const methods = Object.entries(this.stats).map(([name, data]) => ({
      name,
      data,
    }));
    methods.forEach((method, index) => {
      const avg = method.data.total / method.data.times.length;
      const median = this.calculateMedian(method.data.times);
      console.log(`${index + 1}. ${method.name}`);
      console.log(`   Average: ${avg.toFixed(3)}ms`);
      console.log(`   Median: ${median.toFixed(3)}ms`);
      console.log(`   Min: ${method.data.min.toFixed(3)}ms`);
      console.log(`   Max: ${method.data.max.toFixed(3)}ms\n`);
    });
  }
  clear() {
    this.stats = {};
  }
}
exports.PerformanceMeasurer = PerformanceMeasurer;
```

## 2. Performance Benchmark (Object)
```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const _00_test_1 = require("./00-test");
const obj = {};
for (let i = 0; i < 1000000; i++) {
  obj[i.toString()] = i * 2;
}
const iterations = 10000;
const measurer = new _00_test_1.PerformanceMeasurer();

measurer.measure("for...of loop (object values)", () => {
  for (const x of Object.values(obj)) {
    let y = x;
    y = y + 1;
  }
});

measurer.measure("for...in loop (object keys)", () => {
  for (const x in obj) {
    let y = obj[x];
    y = y + 1;
  }
});

measurer.measure("Object.keys() + for loop", () => {
  const keys = Object.keys(obj);
  for (let i = 0; i < keys.length; i++) {
    let y = obj[keys[i]];
    y = y + 1;
  }
});

measurer.measure("Object.entries()", () => {
  for (const [key, value] of Object.entries(obj)) {
    let y = value;
    y = y + 1;
  }
});

measurer.printSummary(
  "Object Loop Performance Statistics",
  iterations,
  Object.keys(obj).length
);
```

```bash
=== Object Loop Performance Statistics ===
Total iterations: 10000
Data size: 1000000

1. for...of loop (object values)
   Average: 13.401ms
   Median: 13.401ms
   Min: 13.401ms
   Max: 13.401ms

2. for...in loop (object keys)
   Average: 46.187ms
   Median: 46.187ms
   Min: 46.187ms
   Max: 46.187ms

3. Object.keys() + for loop
   Average: 66.711ms
   Median: 66.711ms
   Min: 66.711ms
   Max: 66.711ms

4. Object.entries()
   Average: 198.382ms
   Median: 198.382ms
   Min: 198.382ms
   Max: 198.382ms
```

> If you only want to use values, use `Object.values()`, it's the fastest.
> If you want to use both keys and values, use `for...in`, it's the fastest.
{: .prompt-tip}

## 3. Performance Benchmark (Array)
```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.PerformanceMeasurer = void 0;
const _00_test_1 = require("./00-test");
Object.defineProperty(exports, "PerformanceMeasurer", {
  enumerable: true,
  get: function () {
    return _00_test_1.PerformanceMeasurer;
  },
});
const arr = Array.from({ length: 1000000 }, (_, i) => i * 2);
const measurer = new _00_test_1.PerformanceMeasurer();
const iterations = 10000;

measurer.measure(
  "for...of",
  () => {
    for (const x of arr) {
      const y = x;
    }
  },
  iterations
);

measurer.measure(
  "traditional for",
  () => {
    for (let j = 0; j < arr.length; j++) {
      const x = arr[j];
    }
  },
  iterations
);

measurer.measure(
  "forEach",
  () => {
    arr.forEach((x) => {
      const y = x;
    });
  },
  iterations
);

measurer.printSummary(
  "Array Loop Performance Statistics",
  iterations,
  arr.length
);
```
```bash
=== Array Loop Performance Statistics ===
Total iterations: 10000
Data size: 1000000

1. for...of
   Average: 0.590ms
   Median: 0.285ms
   Min: 0.285ms
   Max: 8.291ms

2. traditional for
   Average: 0.312ms
   Median: 0.286ms
   Min: 0.285ms
   Max: 1.286ms

3. forEach
   Average: 1.562ms
   Median: 0.286ms
   Min: 0.285ms
   Max: 11.081ms
```

> I recommend using the `traditional for` loop when the iteration count is huge.
> But it's okay to use `forEach` for more readable code.
{: .prompt-tip}