---
title: "Tail Recursion - The functional loop"
published: false
---

**Calculating prime numbers in a range**

*Note: I wrote this in Intellij using a Scala worksheet.*

```scala
def isPrime(currentNumber: Int, primes: mutable.ArrayBuffer[Int]): Boolean = {
	var isPrime = true
	var i = 0

	while (isPrime && i < primes.length) {
		if (currentNumber % primes(i) == 0) isPrime = false
		i = i + 1
	}
	isPrime
}


def loopPrimes(number: Int): List[Int] = {
	val primes = mutable.ArrayBuffer.empty[Int]

	(2 to number).foreach { number =>
		if (isPrime(number, primes)) primes.append(number)
	}
	primes.toList
}
```

```scala
def recFindPrime(i: Int, ints: List[Int]): List[Int] = {
	def f(primes: List[Int]): List[Int] = primes match {
		case Nil => ints ++ List(i)
		case _ if i % primes.head == 0 => ints
		case _ :: tail => helper(tail)
	}

	f(ints)
}

def recPrimes(number: Int): List[Int] = {
	def f(n: Int, primes: List[Int] = List.empty): List[Int] =
		if (n >= number) {
			primes
		} else {
			helper(n + 1, recFindPrime(n + 1, primes))
		}

	f(1)
}
```
