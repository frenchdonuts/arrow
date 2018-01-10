---
layout: docs
title: Id
permalink: /docs/datatypes/id/
---

## Id

The identity monad can be seen as the ambient monad that encodes the effect of having no effect. 
It is ambient in the sense that plain pure values are values of `Id`.

```kotlin
import arrow.*
import arrow.core.*

Id("hello")
//kotlin.Unit
```

Using this type declaration, we can treat our Id type constructor as a `Monad` and as a `Comonad`. 
The `pure` method, which has type `A -> Id<A>` just becomes the identity function. The `map` method 
from `Functor` just becomes function application

```kotlin
val id: Id<Int> = Id.pure(3)
id.map{it + 3}
//kotlin.Unit
```

Available Instances:

```kotlin
import arrow.debug.*

showInstances<IdHK, Unit>()
//kotlin.Unit
```
