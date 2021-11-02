# Stack Data Structure

## Learning Goals

- Learn what a `Stack` is
- Identify common methods for a `Stack`
- Identify common use cases for a `Stack`

## Introduction

In this lesson, we'll learn what `Stack`s are and what methods they commonly
include. We'll discuss time complexity considerations when using `Stack`s and
provide some common real-world examples of when `Stack`s are used. We'll also
walk through an example algorithm, first coding it without using a `Stack`, and
then with one.

## Defining a Stack

A `Stack` is a linear data structure that allows you to store a list of data of
some sort, and to add and remove values. Values in the stack are processed in
**First In, Last Out** (FILO) order. In other words, the value that was added to
the `Stack` most recently will be the first one removed. This can be contrasted
with another similar data structure, a `Queue`, which is processed in **First In,
First Out** (FIFO) order.

If we consider an airport security checkpoint as a real world example, the stack
of bins is our `Stack`: when a passenger grabs a bin from the stack, it's the
last bin that was added; in other words, **First In, Last Out**. (You can also
think of it as **Last In, First Out**; the two terms are equivalent.) The line
of passengers waiting to pass through security would be our `Queue`: the first
person to join the line will be the first one through the checkpoint
(**First In, First Out**).

It can be useful to think of a `Stack` as a vertical structure, like a stack of
plates: we generally refer to adding items to, and removing them from, the _top_
of the `Stack`:

![Stack](https://curriculum-content.s3.amazonaws.com/phase-4/phase-4-data-structures-stack/stack.png)

### Stack vs. Array

You may be wondering why we wouldn't just use an array instead of implementing a
`Stack`. After all, arrays are also used to store a list of data, and also allow
you to add and remove values. In fact, one way to implement a `Stack` (although
not generally the best way) is by using an array as the underlying data
structure — you'll be doing that in the next lesson.

`Stack`s have several benefits for certain problems when compared to arrays.
`Stack`s have a more limited set of methods for interacting with data compared
to arrays: with a `Stack`, you can only interact with the element at the _top_,
whereas arrays allow you to access and interact with elements at _any_ position.
This restriction is actually a good thing when it comes to solving certain kinds
of problems, since it can guide you to a more elegant and easy-to-understand
solution.

## Stack Methods

The implementation of a `Stack` will vary depending on what's needed, but, at a
minimum, generally includes the following methods:

- `push`: add an element to the top of the stack
- `pop`: remove the element at the top of the stack
- `peek` (or `top`): return the value of the element at the top of the stack
  without removing it

In some implementations, you might also want to include a `limit` attribute,
to indicate the maximum size of the `Stack`.

> Fun Fact: the phrase **stack overflow** was originally coined to describe the
> situation of trying to push an item to a full `Stack` — it isn't just a place
> to find answers to coding questions! The reverse situation — trying to pop
> an item off of an empty `Stack` — is referred to as **stack underflow**.

Some other common methods you might see implemented include:

- `empty?`/`full?`: return true if the `Stack` is empty/full; false
  otherwise
- `search(target)`: return the distance between the top of the stack and the
  target element if it's present; -1 otherwise
- `size`: return the number of elements contained in the `Stack`

Other methods are possible as well, of course: the methods the developer chooses
to define in a given implementation of a `Stack` will depend on their particular
needs.

Note also that there is nothing magical about the names of the methods. The
names listed above are typically used by convention — and, as always, sticking
to convention generally makes your code easier to read for other developers. But
if you have a good reason for breaking convention in a particular circumstance,
there's no reason you can't!

### Time Complexity of Stack Methods

With the exception of `search`, all of the `Stack` methods listed above (for
example, pushing an element onto the `Stack`) have time complexity of O(1). The
time complexity for `search` is O(n).

Let's look at an example use of a `Stack`:

```rb
def reverse_string(string)
  stack = []
  string.chars.each do |char|
    stack.push(char)
  end
  reversed = ""
  until stack.empty? do
    reversed += stack.pop
  end
  reversed
end

reverse_string("hello")
# => "olleh"
```

Here we are iterating through the string and adding each character to the
`Stack`, which has a time complexity of O(n). Then, we loop through the `Stack`,
pop each character off and add it to the reversed string, again yielding a time
complexity of O(n). This gives O(2n), which simplifies to O(n).

## When To Use a Stack

There are a number of practical use cases for a `Stack`. Some common ones include:

- The [call stack][call-stack] in computing
- Code compilers checking if brackets are balanced when a program is run
- Browser history and back/forward buttons
- Undo/redo in software programs

A `Stack` can also be used to help traverse a more complex data structure known
as a `Tree`. (We'll learn about `Tree`s a bit later in this section.) For
example, one common use of `Stack`s is in implementing a depth-first search
through a `Tree`.

### Example

Let's take a look at an example to see how we might use a `Stack` in solving a
problem.

In math, the factorial of an integer `n` (expressed as `n!`) is equal to the
product of all integers from `n` down to 1:

```txt
n! = n * (n-1) * (n-2) ... * 1
```

So the factorial of 4 would be `4 * 3 * 2 * 1`, or 24.

There are many ways to approach computing the factorial of a number; one
possible solution might look like this:

```rb
def factorial(n)
  result = 1
  while n > 1 do
    result *= n
    n -= 1
  end
  result
end

factorial(4)
# => 24
```

Here we are using a while loop to count down from `n` to 1, updating our result
in each iteration by multiplying it by the current value of `n`.

Now let's take a look at how we might instead use a `Stack` to solve this
problem. Although we haven't implemented a `Stack` yet (you'll do that in the
next lesson), we can capture the behavior of one by using the Ruby `Array`
`push` and `pop` methods.

Using a `Stack`, our solution might look like this:

```rb
def factorial(n)
  stack = []
  result = 1
  while n > 1 do
    stack.push(n)
    n -= 1
  end
  until stack.empty? do
    result *= stack.pop
  end
  result
end

factorial(4)
# => 24
```

First, we use a loop to count down from `n` to 1 and `push` each number onto our
stack:

![Pushing to the stack](https://curriculum-content.s3.amazonaws.com/phase-4/phase-4-data-structures-stack/stack-push.png)

Then we use a second loop to `pop` each number in turn off the stack and
multiply it by our running product:

![Popping off the stack](https://curriculum-content.s3.amazonaws.com/phase-4/phase-4-data-structures-stack/stack-pop.png)

Note that we used this example to help get a sense of what a `Stack` is and how
it may be used to solve a problem, not because this is the best way to solve
this particular problem. If you notice, using a stack here did not buy us
anything in terms of time complexity. In fact, this solution is a bit _worse_
than the first one (although not enough to affect its big O). In the initial
solution, we are using a loop to count down from `n` to 1, which gives us a time
complexity of O(n). For our `Stack` solution, we are using **two** loops: one to
add the numbers to the stack and a second to `pop` them back off and calculate
our result. This gives a time complexity of O(2n), which simplifies to O(n).

## Conclusion

In this lesson, we learned about the `Stack` data structure and the methods that
an implementation of a `Stack` usually includes. We also talked about some
real-world use cases for a `Stack` and went through an example algorithm. In the
next lesson, you'll tackle implementing a `Stack`.

## Resources

- [Wikipedia: Stack (abstract data type)][stack]

[stack]: https://en.wikipedia.org/wiki/Stack_(abstract_data_type)
[call-stack]: https://en.wikipedia.org/wiki/Call_stack
