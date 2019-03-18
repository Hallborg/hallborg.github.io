---
title: "Never change - working with immutability"
published: true
---

**Comparing Java, Kotlin and Scala immutability**, all fun and games.

This blog post is meant to give a short comparisons of immutable collections in Java, Kotlin and Scala. As a bonus you will also get familiar with the syntax.

Lets start of by how to initialize an immutable list and store the reference to it.

##### Java
```java
final List<Integer> list = Arrays.asList(1,2,3)
```

##### Scala
```scala
val list = List(1,2,3)
```

##### Kotlin
```kotlin
val list = listOf(1,2,3)
```

The java syntax is quite verbose with the 'List list' statement. The 'final' keyword makes the variable immutable.
For Scala and Kotlin the 'final <T>' has been replaced with 'val'. Now lets see how this will affect our code.

##### Java
```java
// Throws an UnsupportedOperationException due to '.asList(..)' being immutable.
list.add(4);
// Won't compile as 'list' is 'final'.
list = null;
```

##### Scala
```scala
// Won't compile as the List class is immutable. There is no method to make an addition to the list.
list.add(4)
// Won't compile due to reassignment of val.
list = null
```

##### Kotlin
```kotlin
// Won't compile as 'listOf(..)' returns a 'readOnlyList'. There is no method to make an addition to the list.
list.add(4)
// Won't compile due to reassignment of val, and due to null saftey (not allowed to use null).
list = null
```

Things being immutable seems annoying (look at all those errors) so why use it at all?

First of all, often our code does not need to hold a state. We have file systems and databases to ensure that the precious information is not forever gone
if someone pulls the plug.

Data won't change once it's in the flow of the system. From position A -> B the data will be the same. Together with the stateless code, it makes sense to use
an immutable structure for our programs. From A -> B all we as programmers need is to transform A until B is fulfilled. Transformation is just applying functions,
so how do we do that?

```
f(A) ~> B
```
