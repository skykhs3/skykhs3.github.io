---
title: "Structural type system in TypeScript"
date: 2024-10-26 00:00:00 +09:00
categories: [Development, TypeScript]
author: skykhs3
image:
  path: /assets/img/posts/2024-10-26-typescript-duck-typing/typescript.webp
  alt: Typescript
  show_in_post: false
tags:
  - structural type system
  - typescript
description: While debugging legacy TypeScript code, I encountered an odd issue—a field of a object not defined in the interface appeared unexpectedly.
---

## Something unexpected happened in TypeScript code...

<div markdown="1">

```typescript
interface Truck {
  weight: number;
}

interface Bus {
  weight: number;
  color: string;
}

function getTruckAttributes(truck: Truck){
  console.log("A bus can be a truck");
}

let bus: Bus = {"weight":1, "color": "red"};

getTruckAttributes(bus);
```
```bash
[LOG]: "A bus can be a truck" 
```

The above code has no errors at a `getTruckAttributes` function. The code prints "A bus can be a truck", yet the truck has an `Truck` interface and the bus has `Bus` interface.

## This happens because of structural type system 

### A structural type system
[A structural type system](https://en.wikipedia.org/wiki/Structural_type_system){:target="_blank"} (or property-based type system) is a major class of type systems in which type compatibility and equivalence are determined by the type's actual structure or definition and not by other characteristics such as its name or place of declaration.
For the above code, `bus` has the `weight` and `color` attributes, thus it can pass `getTruckAttributes` function as parameter

### A norminal type system
[A type system is nominal](https://en.wikipedia.org/wiki/Nominal_type_system){:target="_blank"} (also called nominative or name-based) if compatibility and equivalence of data types is determined by explicit declarations and/or the name of the types. Nominal systems are used to determine if types are equivalent, as well as if a type is a subtype of another. Nominal type systems contrast with structural systems, where comparisons are based on the structure of the types in question and do not require explicit declarations.


The following C++ code mirrors the logic and structure of the Typescript code but throws an error in the `getTruckAttributes` function. This is because it violates the normal type system in C++.

```c++
#include <string>
#include <iostream>

struct Truck {
  int weight;
};

struct Bus {
  int weight;
  string color;
};

void getTruckAttributes(Truck truck){
  std::cout<<"Bus can't be Truck";
}

int main(){
  Bus bus = {1, "red"};
  getTruckAttributes(bus);
}
```

```bash
main.cpp:19:3: error: no matching function for call to 'getTruckAttributes'
  getTruckAttributes(bus);
  ^~~~~~~~~~~~~~~~~~
main.cpp:13:6: note: candidate function not viable: no known conversion from 'Bus' to 'Truck' for 1st argument
void getTruckAttributes(Truck truck){
     ^
1 error generated.
```

## Why should you consider TypeScript's structural type system?

Overlooking TypeScript's structural type system can lead to logical errors in your code.
``` typescript
interface Truck {
  weight: number;
}

interface Bus {
  weight: number;
  color: string;
}

function getTruckAttributes(truck: Truck){
  let vehicle = {color: "blue", ...truck}; //assign it blue.
  console.log(vehicle.color);
}

let bus: Bus = {"weight":1, "color": "red"};

getTruckAttributes(bus);
```
```bash
[LOG]: "red" 
```

In this example, the `Truck` interface doesn't have a `color` field, so I assigned it a blue. However, the code outputs "red" instead of "blue". This happens because the `truck` parameter includes `color` field.

In this short code snippet, the issue is easy to spot, but in a larger codebase, such an error could be challenging to detect.

## How to solve this problem?
First, be cautious when using the spread operator (...). Keep in mind that the spread operation can overwrite existing properties, and an interface does not guarantee that there won't be additional fields in an object.

Second, consider using a **"tag" field** to ensure type safety. The following example shows how tagging can throw an error and help prevent mistakes.

```typescript
interface Truck {
  tag: 'truck'
  weight: number;
}

interface Bus {
  tag: 'bus'
  weight: number;
  color: string;
}

function getTruckAttributes(truck: Truck){
  console.log("A bus can be a truck");
}

let bus: Bus = {"weight":1, "color": "red"};

getTruckAttributes(bus);
```

```bash
Argument of type 'Bus' is not assignable to parameter of type 'Truck'.
  Types of property 'tag' are incompatible.
    Type '"bus"' is not assignable to type '"truck"'.
'truck' is declared but its value is never read.
```

</div>