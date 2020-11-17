---
layout: post
title:  "TypeScript - Types based on input"
permalink: "ts-01"
date:   2020-11-17 01:00:00 +0000
categories: typescript
---

# Overview

This post is about wrangling TypeScript to return types based on the input.

Lets say we have a function `getAnimalStats(animal)` that gives us data back about an animal. The animals data we get back depends on the string name of the animal we pass in. That means if we pass it "Pig", we should get some data back about a pig.
&nbsp;

In most cases, it would be better to refactor that function into different functions (e.g. `getPigStats()`, `getSheepStats()`), but this post will cover the times where that isn't possible or practical.
&nbsp;

# First Try

We might make use of union types like below.
&nbsp;

```typescript
type Animal = "pig" | "sheep" | "goat";

interface PigStats {
  tailLengthCm: number;
}

interface SheepStats {
  woolLengthCm: number;
}

interface GoatStats {
  hoofLength: number;
}

function getAnimalStats(animal: Animal): PigStats | SheepStats | GoatStats {
    switch(animal) {
        case "pig": return { tailLengthCm: 10 };
        case "sheep": return { woolLengthCm: 10 };
        case "goat": return { hoofLength: 10 };
    }
}

const pigStats = getAnimalStats("pig");
```
[Playground Link][playground-link-1]
&nbsp;

However, when we try to use `pigStats`, we'll get errors:
&nbsp;

```typescript
console.log(pigStats.tailLengthCm); // Property 'tailLengthCm' does not exist on type 'SheepStats'
```
&nbsp;

That's because `pigStats` is of type `PigStats | SheepStats | GoatStats`, not `PigStats`. TypeScript doesn't know of any connection between the `"Pig"` and `PigStats` and the can't guarantee the values exist as you need them.
&nbsp;

So how do we connect strings like `"pig"` to types like `PigStats`? It's not valid TypeScript, but having an object like this would really help, then we could just call `AnimalTypes['pig']`:
&nbsp;

```typescript
const AnimalTypes = {
  "pig": PigStats,
  "sheep": SheepStats,
  "goat": GoatStats
};
```
&nbsp;

Unfortunately, that isn't valid TypeScript, it does however look suspiciously like something that is: an interface!

```typescript
interface AnimalTypes {
  pig: PigStats;
  sheep: SheepStats;
  goat: GoatStats;
};
```
&nbsp;

Right, but interfaces can't be indexed, can they? Even then, we can't access the keys of an interface like we would an object can we?

You may have guessed, but it turns out the answer is "yes we can!", thanks to the [keyof type operator][keyof] and [generics][generics].
&nbsp;

# A Solution

The `keyof` operator will give you a [union type][union-type] of all the keys of a type.

That means if we give it our `AnimalTypes` interface, it will give us a type: `"pig" | "sheep" | "goat"`.

&nbsp;

To specify limitations to a generic type, we can ask it to extend from another type. If we specify our `getAnimalStats` function to use a generic type `T` that extends our above type, `T` would be equal to the constant string type of the passed in string.

Put more simply, if we passed in `"pig"` to our function, our generic type `T` would be the same as `"pig" as "pig"` or `"pig" as const`.
&nbsp;

We can then index `AnimalType` with `T` for our return type! In the `"pig"` example, this means `AnimalType[T]` will be `PigStats`.

Here's the magic in full:
&nbsp;
```typescript
interface AnimalType {
 pig: PigStats;
 sheep: SheepStats;
 goat: GoatStats;
}

interface PigStats {
  tailLengthCm: number;
}

interface SheepStats {
  woolLengthCm: number;
}

interface GoatStats {
  hoofLength: number;
}

function getAnimalStats<T extends keyof AnimalType>(animal: T) {
    switch(animal) {
        case "pig": return { tailLengthCm: 10 } as AnimalType[T];
        case "sheep": return { woolLengthCm: 10 } as AnimalType[T];
        case "goat": return { hoofLength: 10 } as AnimalType[T];
        default: throw new Error("Invalid Animal: " + animal);
    }
}

const pigStats = getAnimalStats("pig");
console.log(pigStats.tailLengthCm);
```
&nbsp;

Thanks,
&nbsp;

l

[playground-link-1]: https://tsplay.dev/lm03oN
[generics]: https://www.typescriptlang.org/docs/handbook/generics.html
[keyof]: https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html
[union-type]: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types
[playground-link-2]: https://tsplay.dev/OwE3vm
