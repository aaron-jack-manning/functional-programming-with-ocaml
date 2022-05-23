## Currying

When using a function, we need to know what type the function has. Let's consider the following function written in C.

```
int square(int x)
{
    return x * x;
}
```

Here we can identify the type by the top of the function declaration: `int square(int x)`. Here the type at the front is the type we return, and the type inside the parenthesis is the input type.

In functional languages we generally write types in a more mathematical way. For the above function we would write:

```
square : int -> int
```

This means that the function square takes in an `int`, and returns an `int`.

When applying functions in many functional languages we also omit the brackets. Therefore:

```
square 5
```

is the equivalent of:

```
square(5);
```

Let's extend the above idea to functions with multiple inputs. In general, we don't technically have multiple inputs, but rather a function that returns a function to take the next input, or many pieces of data grouped as one.

Considering the latter first, if we want to have an argument take in many inputs, what we are actually doing is taking in one argument which is a data structure that groups a few pieces of data together, called a tuple. This tuple can be destructed in the header of the function. For example a function that calculates the distance to the origin of a point may have a top line like this:

```
let distance (x : float, y : float) : float =
```

There is however another option, being currying. Currying is just the ideas that instead of a function taking multiple arguments, it takes the arguments one at a time, and then returns a function to take the next argument. The top line of a curried version of the above function would look like this.

```
let distance (x : float) (y : float) : float =
```

A call to this function would then look like this:

```
distance 5.0 2.0
```

You can think about the parenthesis as left associative here:

```
(distance 5.0) 2.0
```

So distance is a function that takes a `float`, and returns a function that takes a `float` and returns a `float`.