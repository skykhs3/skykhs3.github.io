---
title: "TypeScript: Exhaustiveness Checking"
date: 2025-05-22 16:44:00 +09:00
categories: [Development,  TypeScript]
author: skykhs3
image:
  path: /assets/img/posts/2024-10-26-typescript-duck-typing/typescript.webp
  alt: TypeScript Error
  show_in_post: false
tags:
  - typescript
description: TypeScript's Exhaustiveness Checking helps catch missing cases in switch statements.
---
```json
  "devDependencies": {
    "ts-node": "10.9.2",
    "typescript": "5.8.3"
  }
```
## 1. Why is Exhaustiveness Checking needed?
**Exhaustiveness Checking** is a feature of TypeScript. Let's look at the example below.
![Normal](/assets/img/posts/2025-05-22-typescript-exhaustive-check/00-normal.webp)

It looks perfect. But what happens if **a new color type (black)** is added during code maintenance?
![TS Error](/assets/img/posts/2025-05-22-typescript-exhaustive-check/01-add-black.webp)

We might not realize that we need to add a new case to handle the new type, especially if there are many lines of code.
In a production environment, **we might miss adding a new line in the switch case.**

**Exhaustiveness Checking helps prevent this situation.**

## 2. How to use Exhaustiveness Checking?

![Never](/assets/img/posts/2025-05-22-typescript-exhaustive-check/02-complie-error.webp)
Unlike before, the default case uses `never`. **This generates a compile-time error for values that are not handled in any case.**

If you use the exhaustive check pattern, you just find the TypeScript compile error and add a new case.
![Add](/assets/img/posts/2025-05-22-typescript-exhaustive-check/03-add-black-normal.webp)

You can use this method not only with `switch` statements but also with `if` statements.
![If](/assets/img/posts/2025-05-22-typescript-exhaustive-check/04-if.webp)

## 3. Be Cautious!

This is a Compile Time error! It just notifies you that you missed the logic. **It cannot catch errors at runtime.**

![Runtime](/assets/img/posts/2025-05-22-typescript-exhaustive-check/05-runtime-error.webp)