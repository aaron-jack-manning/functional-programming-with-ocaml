## Loops

Just as any imperative programming language would, OCaml also has loops. If you have come from a background in imperative or object oriented languages, and were new to functional languages with this text, take a moment to appreciate the amount of code we have written without the need for loops. In fact, we are going to continue to write code in that way from this moment forward, as that style is generally prefered.

That said, as we look at imperative programming features of OCaml, it makes sense to look at loops and mutability together, given how we established how well coupled together they are when looking at the first Project Euler problem.

## For

For loops in OCaml are fairly intuitive and straightforward. Here is an example of a `for` loop.

```
for i from 1 to 10 do
    print_int i
done
```

The header of the loop contains a variable which will be iterated over, and a lower and upper bound which are integers. The variable `i` will take on all variables from `1` to `10` *inclusive*.

OCaml `for` loops also have the ability to support an indexing variable which is decreasing, using the `downto` keyword as follows:

```
for i from 10 downto 1 do
    print_int i
done
```

## While

While loops in OCaml are also very similar to those of other languages, and look like the following:

```
while condition do
    (* something *)
done
```

You may notice something interesting about `while` loops with respect to functional programming, which is that the condition can only ever change to evaluate to `false` and halt the execution of the loop if what is determining it is mutable. This is why `while` loops are not generally preferred in OCaml, because they require us to use references so we can mutate the condition, and because the nature of a loop breaking upon a condition at the start yields a very simple recursive equivalent, especially since there is no support in OCaml for `break` and `continue`.

Hence the use case for `while` loops is very minimal and are mostly avoided, but are presented here for completeness. This is not true of for loops, which can genuinely be very useful to produce more readable code than recursion for nested loops.
