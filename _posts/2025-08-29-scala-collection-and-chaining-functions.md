---
title: "Scala: Collection and Chaining Functions"
date: 2025-08-29 00:28:00 +09:00
categories: [Languages, Scala]
author: skykhs3
image:
  path: /assets/img/posts/scala-logo.webp
  alt: scala logo
  show_in_post: false
tags:
  - scala
description: A comprehensive guide to Scala collections including Option, List, and Map with examples of map, flatten, and flatMap operations.
---

| Item | Version |
|-|-|
| Scala | 2.12.17 |

## 1. Option

### 1.1. get
```scala
Some(10).get // 10
// println(noneValue.get) // throws NoSuchElementException
None.orElse(None).getOrElse(11) // 11
```

### 1.2. flatten
```scala
Some(Some(10)).flatten // Some(10)
Some(Some(None)).flatten // Some(None)
Some(None).flatten // None
None.flatten // None
```

### 1.3. map
```scala
Some(10).map(_ * 2) // Some(20)
None.asInstanceOf[Option[Int]].map(_ * 2) // None
```

### 1.4. flatMap
```scala
Some(Some(10)).flatMap(tripleOption) // Some(30)
Some(None).flatMap(tripleOption) // None
```

### 1.5. toList

```scala
Some(10).toList // List(10)
None.toList // List()
```

## 2. List

### 2.1. map
```scala
List(1, 2).map(_ * 2) // List(2, 4)
List(Option(1), None).map(tripleOption) // List(Some(3), None)
```

### 2.2. map, flatten, and flatMap
```scala
val tripleOption: Option[Int] => Option[Int] = {
  case Some(x) => Some(x * 3)
  case None => None
}

val tripleIterable: Option[Int] => Iterable[Int] = {
  case Some(x) => Some(x * 3)
  case None => None
}

List(1, 2).map(_ * 2) // List(2, 4)
List(Some(1), Some(2), None).map(tripleOption) // List(Some(3), Some(6), None)
List(Some(1), Some(2), None).flatten // List(1, 2)
List(Some(1), Some(2), None).map(tripleOption).flatten // List(3, 6)
List(Some(1), Some(2), None).flatMap(tripleIterable) // List(3, 6)
```

## 3. Map
### 3.1. get
```scala
val numberWords = Map(1 -> "one", 2 -> "two", 3 -> "three")
numberWords.get(1) // Some(one)
numberWords.get(4) // None
numberWords.getOrElse(4, "four") // four
```