---
title: "Tail Recursion - The functional loop"
published: true
---

**Calculating the factorial of a number**

*Note: I wrote this in Intellij using a Scala worksheet.*

 We need to write a method in Scala that returns the factorial of a positive integer.

The first approach might be imperative and look something like this.

***Loop***

```scala
def loopFact(number: Int): Int= {
	var sum = 1
	for (i <- 1 to number) {
		sum *= i
	}
	sum
}
```

1. It starts with initializing a mutable Int 'sum' to 1.
2. Then loop over the number and increase 'sum' by multiply itself with i.
3. Once that is done, return 'sum'.   

These sorts of methods are good to introduce the recursive concepts as they are small and repetitive. Recursive methods tend to in a more declarative style which suits to a functional style of writing code.

So lets have a look on how this can be done.  

***Recursion***

```scala
def recFact(number: Int): Int =
	if (number <= 1) 1
	else number * recFact(number -1)
```

This method operates entirely different from the first one.

1. Any number LTE to 1 should be the last call to itself, return 1
2. Multiply the number with the return value of itself for the number - 1.

I believe that this solution is cleaner, but it has a huge impact on the memory usage.
When a method is called the program needs to know where to continue after the call is completed. Each call to `recFact` is therefor put on the stack.

This can be view from the code using the Thread class

```scala
def recFact(number: Int): Int =
	if (number <= 1) {
		Thread.currentThread.getStackTrace.foreach(println)
		1
	}
	else number * recFact(number -1)
```

which will yield something similar to this for `recFact(3)`
```
$line157.$read$$iw$$iw$.recFact(<console>:15) // will return 1
$line157.$read$$iw$$iw$.recFact(<console>:18) // will return 2 * 1
$line157.$read$$iw$$iw$.recFact(<console>:18) // will return 3 * 2
```


We will get a stack overflow exception if the number to recFact is to high.
`recFact(50000)` will never print, as it tries to push beyond the stack limit.   

Recursion seems to be a bad choice. Unless it wasn't for tail recursion.


If the last thing a method does is a call to itself, the method is tail recursive.
We can force a method to be tail recursive by annotating it with `tailrec`

`scala.annotation.tailrec`
> A method annotation which verifies that the method will be compiled
   with tail call optimization.
  If it is present, the compiler will issue an error if the method cannot
  be optimized into a loop.


Annotating `recFact` with `tailrec` will indeed result in a compilation error.
> Error: could not optimize @tailrec annotated method recFact: it contains a recursive call not in tail position
number * recFact(number - 1)

 The last thing `recFact` does is to calculate the product of the two factors `number` and `recFact(number - 1)`. The method needs to do that calculation *before* calling itself.

***Tail recursion***


```scala
@tailrec
def tailrecFact(number:Int, partialProduct: Int = 1): Int =
	if (number <= 1) partialProduct
	else tailrecFact(number - 1, partialProduct * number)

```

1. Any number LTE to 1 should be the last call to itself, return the product.
2. Call itself with the reduced number, and calculate the new product and pass it as the second parameter.

This is similar to `recFact`, but has a huge impact. The key to making `tailrec` methods is to parameterize the result of the calculation. Once the if-statement is true, partialProduct will be the finalized product.

If the compiler did not automatically notice that this was a tail recursive method it would have look the same as `refFact`. Lets imagine that is the case with `tailrecFact(3)`.

```
$line8.$read$$iw$$iw$.tailrecFact(<console>:16) // will return 6
$line8.$read$$iw$$iw$.tailrecFact(<console>:16) // will return 6
$line8.$read$$iw$$iw$.tailrecFact(<console>:16) // will return 6
```

Since partialProduct is returned on completion no other value will be returned. The final calculation will always result in the end product. Only one stack frame is needed!

When adding the println to this method, you will see that the stack only has 1 `tailrecFact`.

```scala
@tailrec
def tailrecFact(number:Int, partialProduct: Int = 1): Int =
	if(number <= 1) {
    Thread.currentThread.getStackTrace.foreach(println)
    partialProduct
  }
	else tailrecFact(number - 1, partialProduct * number)
```

```
$line8.$read$$iw$$iw$.tailrecFact(<console>:16)
```

We now have a small quite complex method that will be compiled down to a loop, similar to `loopFact`.

A future post will take up the performance of these sort of methods.  
