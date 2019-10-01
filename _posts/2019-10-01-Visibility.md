---
title: "visibility - "
published: false
---

**Keep your implementation to yourself**

In Java; classes can be modified with 'public' or 'package-private' to control access.
(see https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

In the public case, the class can be reached from everywhere.
In the package-private case, only classes within the same package can reach it.

Many might not think that this is not a big deal. Just keep everything public and everyone
can use it! This comes with a big drawback, you will leak implementation details or make
your code harder to test.


*Klass som läcker implementation*

*Klass som är svår att testa*

*Klass med package-private som löser de två översta fallen*
