---
layout: docs
title: SequenceKW
permalink: /docs/datatypes/sequencekw/
---

## SequenceKW

SequenceKW implements lazy lists representing lazily-evaluated ordered sequence of homogenous values.

It can be created from Kotlin Sequence type with a convenient `k()` function.

```kotlin
import arrow.*
import arrow.data.*

sequenceOf(1, 2, 3).k()
//kotlin.Unit
```

SequenceKW derives many useful typeclasses. For instance, it has a [`SemigroupK`](/docs/typeclasses/semigroupk/) instance.

```kotlin
val hello = sequenceOf('h', 'e', 'l', 'l', 'o').k()
val commaSpace = sequenceOf(',', ' ').k()
val world = sequenceOf('w', 'o', 'r', 'l', 'd').k()

hello.combineK(commaSpace.combineK(world)).toList() == hello.combineK(commaSpace).combineK(world).toList()
//kotlin.Unit
```

[`Functor`](/docs/typeclasses/functor/)

Transforming a sequence:
```kotlin
val fibonacci = generateSequence(0 to 1) { it.second to it.first + it.second }.map { it.first }.k()
fibonacci.map { it * 2 }.takeWhile { it < 10 }.toList()
//kotlin.Unit
```

[`Applicative`](/docs/typeclasses/applicative/)

Applying a sequence of functions to a sequence:
```kotlin
SequenceKW.applicative().ap(sequenceOf(1, 2, 3).k(), sequenceOf({ x: Int -> x + 1}, { x: Int -> x * 2}).k()).toList()
//kotlin.Unit
```

SequenceKW is a [`Monad`](/docs/_docs/typeclasses/monad/) too. For example, it can be used to model non-deterministic computations. (In a sense that the computations return an arbitrary number of results.)

```kotlin
import arrow.typeclasses.*

val positive = generateSequence(1) { it + 1 }.k() // sequence of positive numbers
val positiveEven = positive.filter { it % 2 == 0 }.k()

SequenceKW.monad().binding {
   val p = positive.bind()
   val pe = positiveEven.bind()
   yields(p + pe)
}.ev().take(5).toList()
//kotlin.Unit
```

Folding a sequence,

```kotlin
sequenceOf('a', 'b', 'c', 'd', 'e').k().foldLeft("") { x, y -> x + y }
//kotlin.Unit
```

Available Instances:

```kotlin
import arrow.debug.*

showInstances<SequenceKWHK, Unit>()
//kotlin.Unit
```
